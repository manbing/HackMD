---
title: Linux Networking
tags: [Linux]

---

# Linux Networking

``` c
/**
 *  netif_receive_skb - process receive buffer from network
 *  @skb: buffer to process
 *
 *  netif_receive_skb() is the main receive data processing function.
 *  It always succeeds. The buffer may be dropped during processing
 *  for congestion control or by the protocol layers.
 *
 *  This function may only be called from softirq context and interrupts
 *  should be enabled.
 *
 *  Return values (usually ignored):
 *  NET_RX_SUCCESS: no congestion
 *  NET_RX_DROP: packet was dropped
 */
```


## Ethernet Driver
`struct net_device_ops` 不包含「接收」回呼函數（如 `ndo_receive_packet`），是因為在 Linux 核心架構中，==接收封包是由驅動程式「主動推送」給核心，而非由核心「被動要求」驅動程式提供==。
具體原因與機制如下：
1. 數據流向的本質差異
發送（Transmit）：由核心（協定疊）發起。當應用程式想傳送資料，核心需要調用驅動程式的 `ndo_start_xmit` 來操作硬體。因此，這是一個定義在 ops 中的「由上而下」的回呼。接收（Receive）：由外部網路發起。封包隨時可能到達，核心無法預知何時需要「接收」。因此，驅動程式必須在中斷或 NAPI 輪詢期間，主動調用核心提供的 API（如 `netif_rx()` 或 `netif_receive_skb()`）將封包交給核心。
2. NAPI 與效能機制
如果 `net_device_ops` 包含一個接收回呼，核心就必須不斷輪詢所有網卡，這在低負載時非常浪費 CPU 資源。
* 非 NAPI 模式：驅動程式在中斷處理程序中直接呼叫 `netif_rx()`。
* NAPI 模式：驅動程式註冊一個特定的 poll 函數（透過 `netif_napi_add`）。雖然這也是一個回呼，但它不屬於 `net_device_ops`，而是屬於特定的 NAPI 實例，由核心調度器根據負載情況觸發。 ==NAPI is a mechanism that helps mitigate interrupt storms by switching from interrupt-driven packet processing to polling during high-load scenarios==.
3. 責任歸屬
在接收端，驅動程式負責：從硬體接收緩衝區（Ring Buffer）讀取原始數據。將其封裝成核心通用的 `struct sk_buff (skb)`。主動調用核心 API 將 skb 送入協定疊進行後續處理（如 IP、TCP/UDP 解析）。

總結： `net_device_ops` 定義的是核心對設備的「控制權」（如打開、關閉、發送、設定 MAC），而接收封包則屬於「數據輸入」，由驅動程式根據硬體事件自發完成。
### Non-NAPI


### [NAPI](https://docs.kernel.org/networking/kapi.html)
NAPI (New API) is a mechanism used in the Linux kernel for high-speed network packet processing that is a specific implementation of the "bottom half" concept. It is designed to improve performance by mitigating the number of hardware interrupts (top half) at high packet rates. 
#### The Problem NAPI Solves
In the traditional interrupt-driven model, a hardware interrupt is generated for every incoming network packet. For high-speed networks, this "interrupt storm" consumes significant CPU cycles, leading to system performance degradation. 
#### How NAPI Uses the Bottom Half
NAPI switches the processing model from purely interrupt-driven to a hybrid interrupt + polling approach to handle the "bottom half" work efficiently. 
The process flow is as follows:
1. Top Half (Hardware IRQ):
A packet arrives at the network card (NIC) and is placed in the Rx ring buffer via DMA.
The NIC issues an interrupt to the CPU. This is the "top half" of the interrupt handling.
The interrupt handler (the top half code) performs only the most time-critical tasks: it disables/masks further individual interrupts for that specific NIC queue, and then schedules the "bottom half" for deferred processing by calling napi_schedule().
The top half then exits quickly, allowing other hardware interrupts to be serviced.
2. Bottom Half (Softirq/Polling):
The NAPI processing is scheduled as a software interrupt (specifically NET_RX_SOFTIRQ).
The kernel's net_rx_action function, running in a softirq context or a dedicated ksoftirqd kernel thread, then enters a polling loop to "harvest" multiple packets from the NIC's ring buffer.
The driver's poll function is called repeatedly to process a budget of packets (e.g., 64 packets or a time limit) in a single burst. This batched processing amortizes the cost of context switching.
3. Completion:
Once the poll function has processed all available packets (or reached its budget/time limit), the NAPI instance is deactivated (napi_complete()) and the NIC's interrupt mechanism is re-enabled to signal future packet arrivals. 

By deferring the bulk of packet processing (protocol handling, delivery to the IP stack, etc.) to the bottom half and using polling to do so in batches, NAPI drastically reduces interrupt overhead and improves system throughput.

#### Example, typhoon

`linux-5.4.55/drivers/net/ethernet/3com/typhoon.c`:
``` c
module_init(typhoon_init);

static struct pci_driver typhoon_driver = {
    .name       = KBUILD_MODNAME,
    .id_table   = typhoon_pci_tbl,
    .probe      = typhoon_init_one,
    .remove     = typhoon_remove_one,
#ifdef CONFIG_PM
    .suspend    = typhoon_suspend,
    .resume     = typhoon_resume,
#endif
};

static int __init
typhoon_init(void)
{
    return pci_register_driver(&typhoon_driver);
}

static const struct net_device_ops typhoon_netdev_ops = {
    .ndo_open       = typhoon_open,
    .ndo_stop       = typhoon_close,
    .ndo_start_xmit     = typhoon_start_tx,
    .ndo_set_rx_mode    = typhoon_set_rx_mode,
    .ndo_tx_timeout     = typhoon_tx_timeout,
    .ndo_get_stats      = typhoon_get_stats,
    .ndo_validate_addr  = eth_validate_addr,
    .ndo_set_mac_address    = eth_mac_addr,
};


static int
typhoon_init_one(struct pci_dev *pdev, const struct pci_device_id *ent)
{
    dev = alloc_etherdev(sizeof(*tp));
    dev->irq = pdev->irq;
    dev->netdev_ops     = &typhoon_netdev_ops;
    netif_napi_add(dev, &tp->napi, typhoon_poll, 16);
    err = register_netdev(dev);
    
}

static int
typhoon_open(struct net_device *dev)
{
    err = request_irq(dev->irq, typhoon_interrupt, IRQF_SHARED, dev->name, dev);
    napi_enable(&tp->napi);
    netif_start_queue(dev);
}

static irqreturn_t
typhoon_interrupt(int irq, void *dev_instance)
{
    intr_status = ioread32(ioaddr + TYPHOON_REG_INTR_STATUS);
    if(!(intr_status & TYPHOON_INTR_HOST_INT))
        return IRQ_NONE;
    
    iowrite32(intr_status, ioaddr + TYPHOON_REG_INTR_STATUS);
    
    if (napi_schedule_prep(&tp->napi)) {
        __napi_schedule(&tp->napi);
    } else {
         netdev_err(dev, "Error, poll already scheduled\n");
    }
    
    return IRQ_HANDLED;
}

static int
typhoon_poll(struct napi_struct *napi, int budget)
{
    work_done += typhoon_rx(tp, &tp->rxHiRing, &indexes->rxHiReady,
                             &indexes->rxHiCleared, budget);
    
    return work_done;
}

static int
typhoon_rx(struct typhoon *tp, struct basic_ring *rxRing, volatile __le32 * ready,
       volatile __le32 * cleared, int budget)
{
    int received = 0;
    netif_receive_skb(new_skb);
    received++;
    return received;
}
```# Linux Networking

``` c
/**
 *  netif_receive_skb - process receive buffer from network
 *  @skb: buffer to process
 *
 *  netif_receive_skb() is the main receive data processing function.
 *  It always succeeds. The buffer may be dropped during processing
 *  for congestion control or by the protocol layers.
 *
 *  This function may only be called from softirq context and interrupts
 *  should be enabled.
 *
 *  Return values (usually ignored):
 *  NET_RX_SUCCESS: no congestion
 *  NET_RX_DROP: packet was dropped
 */
```


## Ethernet Driver
`struct net_device_ops` 不包含「接收」回呼函數（如 `ndo_receive_packet`），是因為在 Linux 核心架構中，==接收封包是由驅動程式「主動推送」給核心，而非由核心「被動要求」驅動程式提供==。
具體原因與機制如下：
1. 數據流向的本質差異
發送（Transmit）：由核心（協定疊）發起。當應用程式想傳送資料，核心需要調用驅動程式的 `ndo_start_xmit` 來操作硬體。因此，這是一個定義在 ops 中的「由上而下」的回呼。接收（Receive）：由外部網路發起。封包隨時可能到達，核心無法預知何時需要「接收」。因此，驅動程式必須在中斷或 NAPI 輪詢期間，主動調用核心提供的 API（如 `netif_rx()` 或 `netif_receive_skb()`）將封包交給核心。
2. NAPI 與效能機制
如果 `net_device_ops` 包含一個接收回呼，核心就必須不斷輪詢所有網卡，這在低負載時非常浪費 CPU 資源。
* 非 NAPI 模式：驅動程式在中斷處理程序中直接呼叫 `netif_rx()`。
* NAPI 模式：驅動程式註冊一個特定的 poll 函數（透過 `netif_napi_add`）。雖然這也是一個回呼，但它不屬於 `net_device_ops`，而是屬於特定的 NAPI 實例，由核心調度器根據負載情況觸發。 ==NAPI is a mechanism that helps mitigate interrupt storms by switching from interrupt-driven packet processing to polling during high-load scenarios==.
3. 責任歸屬
在接收端，驅動程式負責：從硬體接收緩衝區（Ring Buffer）讀取原始數據。將其封裝成核心通用的 `struct sk_buff (skb)`。主動調用核心 API 將 skb 送入協定疊進行後續處理（如 IP、TCP/UDP 解析）。

總結： `net_device_ops` 定義的是核心對設備的「控制權」（如打開、關閉、發送、設定 MAC），而接收封包則屬於「數據輸入」，由驅動程式根據硬體事件自發完成。
### Non-NAPI


### [NAPI](https://docs.kernel.org/networking/kapi.html)
NAPI (New API) is a mechanism used in the Linux kernel for high-speed network packet processing that is a specific implementation of the "bottom half" concept. It is designed to improve performance by mitigating the number of hardware interrupts (top half) at high packet rates. 
#### The Problem NAPI Solves
In the traditional interrupt-driven model, a hardware interrupt is generated for every incoming network packet. For high-speed networks, this "interrupt storm" consumes significant CPU cycles, leading to system performance degradation. 
#### How NAPI Uses the Bottom Half
NAPI switches the processing model from purely interrupt-driven to a hybrid interrupt + polling approach to handle the "bottom half" work efficiently. 
The process flow is as follows:
1. Top Half (Hardware IRQ):
A packet arrives at the network card (NIC) and is placed in the Rx ring buffer via DMA.
The NIC issues an interrupt to the CPU. This is the "top half" of the interrupt handling.
The interrupt handler (the top half code) performs only the most time-critical tasks: it disables/masks further individual interrupts for that specific NIC queue, and then schedules the "bottom half" for deferred processing by calling napi_schedule().
The top half then exits quickly, allowing other hardware interrupts to be serviced.
2. Bottom Half (Softirq/Polling):
The NAPI processing is scheduled as a software interrupt (specifically NET_RX_SOFTIRQ).
The kernel's net_rx_action function, running in a softirq context or a dedicated ksoftirqd kernel thread, then enters a polling loop to "harvest" multiple packets from the NIC's ring buffer.
The driver's poll function is called repeatedly to process a budget of packets (e.g., 64 packets or a time limit) in a single burst. This batched processing amortizes the cost of context switching.
3. Completion:
Once the poll function has processed all available packets (or reached its budget/time limit), the NAPI instance is deactivated (napi_complete()) and the NIC's interrupt mechanism is re-enabled to signal future packet arrivals. 

By deferring the bulk of packet processing (protocol handling, delivery to the IP stack, etc.) to the bottom half and using polling to do so in batches, NAPI drastically reduces interrupt overhead and improves system throughput.

#### Example, typhoon

`linux-5.4.55/drivers/net/ethernet/3com/typhoon.c`:
``` c
module_init(typhoon_init);

static struct pci_driver typhoon_driver = {
    .name       = KBUILD_MODNAME,
    .id_table   = typhoon_pci_tbl,
    .probe      = typhoon_init_one,
    .remove     = typhoon_remove_one,
#ifdef CONFIG_PM
    .suspend    = typhoon_suspend,
    .resume     = typhoon_resume,
#endif
};

static int __init
typhoon_init(void)
{
    return pci_register_driver(&typhoon_driver);
}

static const struct net_device_ops typhoon_netdev_ops = {
    .ndo_open       = typhoon_open,
    .ndo_stop       = typhoon_close,
    .ndo_start_xmit     = typhoon_start_tx,
    .ndo_set_rx_mode    = typhoon_set_rx_mode,
    .ndo_tx_timeout     = typhoon_tx_timeout,
    .ndo_get_stats      = typhoon_get_stats,
    .ndo_validate_addr  = eth_validate_addr,
    .ndo_set_mac_address    = eth_mac_addr,
};


static int
typhoon_init_one(struct pci_dev *pdev, const struct pci_device_id *ent)
{
    dev = alloc_etherdev(sizeof(*tp));
    dev->irq = pdev->irq;
    dev->netdev_ops     = &typhoon_netdev_ops;
    netif_napi_add(dev, &tp->napi, typhoon_poll, 16);
    err = register_netdev(dev);
    
}

static int
typhoon_open(struct net_device *dev)
{
    err = request_irq(dev->irq, typhoon_interrupt, IRQF_SHARED, dev->name, dev);
    napi_enable(&tp->napi);
    netif_start_queue(dev);
}

static irqreturn_t
typhoon_interrupt(int irq, void *dev_instance)
{
    intr_status = ioread32(ioaddr + TYPHOON_REG_INTR_STATUS);
    if(!(intr_status & TYPHOON_INTR_HOST_INT))
        return IRQ_NONE;
    
    iowrite32(intr_status, ioaddr + TYPHOON_REG_INTR_STATUS);
    
    if (napi_schedule_prep(&tp->napi)) {
        __napi_schedule(&tp->napi);
    } else {
         netdev_err(dev, "Error, poll already scheduled\n");
    }
    
    return IRQ_HANDLED;
}

static int
typhoon_poll(struct napi_struct *napi, int budget)
{
    work_done += typhoon_rx(tp, &tp->rxHiRing, &indexes->rxHiReady,
                             &indexes->rxHiCleared, budget);
    
    return work_done;
}

static int
typhoon_rx(struct typhoon *tp, struct basic_ring *rxRing, volatile __le32 * ready,
       volatile __le32 * cleared, int budget)
{
    int received = 0;
    netif_receive_skb(new_skb);
    received++;
    return received;
}
```