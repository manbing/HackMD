# NAPI
NAPI (New API) is a mechanism used in the Linux kernel for high-speed network packet processing that is a specific implementation of the "bottom half" concept. It is designed to improve performance by mitigating the number of hardware interrupts (top half) at high packet rates. 
## The Problem NAPI Solves
In the traditional interrupt-driven model, a hardware interrupt is generated for every incoming network packet. For high-speed networks, this "interrupt storm" consumes significant CPU cycles, leading to system performance degradation. 
## How NAPI Uses the Bottom Half
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