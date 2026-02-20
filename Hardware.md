---
title: Hardware
tags: [Computer Architecture]

---

# [Bus (computing)](https://en.wikipedia.org/wiki/Bus_(computing))
In computer architecture, a bus (historically also called a data highway[1] or databus) is a communication system that transfers data between components inside a computer or between computers.[2] It encompasses both hardware (e.g., wires, optical fiber) and software, including communication protocols.[3] At its core, a bus is a shared physical pathway, typically composed of wires, traces on a circuit board, or busbars, that allows multiple devices to communicate. To prevent conflicts and ensure orderly data exchange, buses rely on a communication protocol to manage which device can transmit data at a given time.

Buses are categorized based on their role, such as system buses (also known as internal buses, internal data buses, or memory buses) connecting the CPU and memory. Expansion buses, also called peripheral buses, extend the system to connect additional devices, including peripherals. Examples of widely used buses include PCI Express (PCIe) for high-speed internal connections and Universal Serial Bus (USB) for connecting external devices.


# [SPI](https://wiki.csie.ncku.edu.tw/embedded/SPI)
SPI 主控端（Master）連接的從屬裝置（Slave）數量沒有硬性上限，主要受限於 Master 晶片引腳數（需要為每個 Slave 準備一個 Chip Select/SS 引腳）和主板佈線能力，但性能會隨著 Slave 增加而下降，因為需要依次選中每個 Slave，且總線負載會增加。實際應用中，通常會透過增加 Chip Select (CS/SS) 訊號線來連接多個從屬元件，以實現與多個外設的通信。 

# [I2C](https://wiki.csie.ncku.edu.tw/embedded/I2C?revision=642383b3b702954e5f3a46beb8d894dd11086fe3)
An I2C master can theoretically connect to 128 devices (7-bit addressing) or 1024 devices (10-bit addressing), but the practical maximum is limited by total bus capacitance (around 400pF), which affects signal integrity, requiring careful selection of pull-up resistors and potentially slower speeds for many devices or longer distances. The actual limit depends heavily on the specific chips, bus speed (100kHz, 400kHz, 1MHz+), cable length, and pull-up resistor values, with bus extenders helping for longer distances. 


Further Reading: [spi i2c difference](https://www.google.com/search?q=spi+i2c+difference&oq=spi+i2c+difference&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIKCAEQABgFGBMYHjIKCAIQABgIGBMYHjIKCAMQABgIGBMYHjIKCAQQABgIGBMYHjIKCAUQABgIGBMYHjIKCAYQABgIGBMYHjIHCAcQABjvBTIKCAgQABiABBiiBDIKCAkQABiABBiiBNIBCDcxOTlqMGo3qAIIsAIB8QWr39-s2WLOIQ&sourceid=chrome&ie=UTF-8)

# Bit-Banging
using a group of flexible GPIO pins on a microcontroller or development board (like Raspberry Pi) to mimic standard communication buses (like I2C, SPI, or even parallel buses) through software bit-banging, allowing it to communicate with various peripherals that expect these protocols, often for simpler or custom interfaces. It involves configuring pins as outputs to send data and inputs to receive, sometimes requiring specific software drivers or careful pin management for different protocols like 1-Wire or PS/2, providing flexibility but potentially slower than dedicated hardware buses.


# GPIO

# PHY (Physical Layer)
PHY (Physical Layer) refers to the hardware component or circuitry that handles the actual transmission and reception of raw data bits over a physical medium (like copper wire, fiber optic, or airwaves), converting digital data into electrical, optical, or radio signals and vice back, bridging the digital world (MAC Layer) and the physical cable. It's crucial for basic connectivity, managing signal conditioning, encoding/decoding, and link training, ensuring stable data transfer. I.e., *Ethernet PHY*, *USB PHY*,

# MDIO
MDIO (Management Data Input/Output) is a 2-wire serial bus defined by IEEE 802.3 (Clause 22/45) for managing Ethernet Physical Layer (PHY) devices. It consists of a Management Data Clock (MDC) and data line (MDIO), enabling a Master (MAC) to read/write registers in up to 32 slave devices to control link speed, duplex mode, and power. 

*MDC (Clock)*: Driven by the MAC to the PHY to synchronize data.
*MDIO (Data)*: Bidirectional data line, allowing the MAC to write to and read from PHY registers.

Control bus.
[Management Data Input/Output](https://en.wikipedia.org/wiki/Management_Data_Input/Output)


# MII
Data bus.
[Media-independent interface](https://en.wikipedia.org/wiki/Media-independent_interface)

# DTS

# Level Trigger / Edge Trigger
[Interrupt](https://en.wikipedia.org/wiki/Interrupt#Triggering_methods)

# DRAM
## Double data rate (DDR)
[Double data rate](https://en.wikipedia.org/wiki/Double_data_rate)
> In computing, double data rate (DDR) describes a computer bus that transfers data on both the rising and falling edges of the clock signal and hence doubles the memory bandwidth by transferring data twice per clock cycle.[1][2] This is also known as *double pumped*, *dual-pumped*, and *double transition*.
> 
> The term toggle mode is used in the context of NAND flash memory.A comparison between `single data rate`, `double data rate`, and `quad data rate`. The dots are where data transfers take place, measured in millions of transfers per second (MT/s).



## Theoretical DDR bandwidth
Theoretical DRAM bandwidth is calculated by multiplying the transfer rate `(MT/s)` by the bus width `(bytes)` and the number of channels. The standard formula for peak theoretical bandwidth is:
> Transfer Rate (MT/s) x 8 bytes (64-bit) (memory bus width) x Number of Channels


This produces the maximum throughput in MB/s or GB/s, typically assuming 64-bit width per channel. 

For example:
$6000 (MT/s) \times 64 (Bits) \times 1 (channel) = 384000 (Bits/s) \approx 44 (GB/s)$

``` console
$ sudo dmidecode -t memory | grep -A16 'Memory Device' | grep 'Size'

        Size: No Module Installed
        Size: 16 GB
        Size: No Module Installed
        Size: 16 GB

$ sudo dmidecode -t memory | grep -A16 'Memory Device' | grep 'Speed'

        Speed: 6000 MT/s
        Speed: 6000 MT/s
        
$ sudo dmidecode -t memory | grep -A16 'Memory Device' | grep 'Data Width'
        Data Width: Unknown
        Data Width: 64 bits
        Data Width: Unknown
        Data Width: 64 bits
        
$ sudo dmidecode -t memory | grep Channel
```

# 32/64 Bit Architecture
The 32/64 Bit is meaning register size or bus width?. 32 bits address bus can address 2^{32} memory address; 64 bits address bus can address 2^{64} memory address.

The term “64-bit machine” is vague. Computers processors and systems have several features which may have sizes different from each other on the same machine, including:
* The width of most processor registers.
* The width of addresses.
* The width of the data bus.
* The width of the arithmetic logic units.

# Memory Module
## Dual In-line Memory Module (DIMM)
[DIMM](https://en.wikipedia.org/wiki/DIMM)

## Memory Read
資料流動過程 (Data Flow)
1. Core 發出讀取請求，L1 Cache 沒中 (Miss)。
2. 請求被掛載到一個空的 Line Full Buffer (LFB)。
3. LFB 詢問 L2/L3，若都沒中，請求轉發給 Integrated Memory Controller (IMC)。
3. IMC 從 RAM 搬回 64 Bytes 的資料片段。
4. LFB 接收碎片並湊齊一個完整 Line。
5. LFB 將資料寫入 L1 並通知 Core：資料準備好了！

# Core
## Core Bandwidth
## 單核心記憶體頻寬 (受限於延遲)
雖然 DDR5-6000 的總頻寬可達 48 GB/s，但單個核心通常無法跑滿這個數值。這是因為單核心受限於「未完成請求」(Concurrency) 的硬體緩衝區限制（Little's Law）。 

參數（典型值）：
* Memory Latency (DDR4)：around 15 ns
* Cache Line Size：64 Bytes
* Line Fill Buffer (LFB)（Little's Law）：約 10–12 個
* 公式：(Line Fill Buffer × Cache Line Size) / Memory Latency
* 計算：$(12 \times 64 Bytes) \div 15 ns \approx 47.6 GB/s$
 
Yes! It needs **two** core to saturate Memory bandwidth.

這是一個非常深刻的觀察！在計算記憶體頻寬或延遲時，我們似乎「忽略」了 CPU 的時脈（例如 5.0 GHz），這背後有幾個關鍵的原因：

**CPU 和記憶體運行在完全不同的時鐘域（Clock Domains）中。**

* CPU： 運行速度極快（週期約 0.2 ns）。
* 記憶體總線： 運行速度相對慢得多。
* 緩衝區的作用： 當 CPU 需要資料時，它會將請求丟入 LFB (Line Fill Buffers)。一旦請求離開了 CPU 核心進入記憶體控制器（IMC），它就進入了「等待物理傳輸」的階段。此時，CPU 的時脈再快，也無法加速電子在 PCB 板上的傳輸速度或 DRAM 電容的充放電。


# Chip V.S. Processor V.s Core
[Multi-core processor](https://en.wikipedia.org/wiki/Multi-core_processor)

One **System on a Chip (SoC)** be composed of Multiple **Processor** and **Memory**.
One **Processor (Processor Chip) (CPU)** be composed of Multiple **Core** and **L2 and L3 cache**.
**Core** is the Smallest unit of execution and be composed of **L1 cache**.

* **System on a Chip (SoC)**: Combines a processor (CPU), main memory (RAM), input/output controllers, and sometimes graphics (GPU) onto one chip.

* **Processor Chip (CPU)**: Primarily designed for logic and arithmetic, though they often contain small, fast "cache" memory on-die, rather than the large capacity "main" memory.

* **Traditional PC Architecture**: In many desktop computers, the CPU and main memory are physically separate chips on the motherboard, connected via a bus. 


$$
Chip > Processor > Core
$$

# Cache
## Cache Concurrency
managing simultaneous read/write access to cached data by multiple threads or processes.

Cache 硬體並行度不足主要發生在 Set Associative Cache 中，當 Way（路）數過多但缺乏足夠的比較器（Comparator）同時比對 Tag 時。高組相聯（High-way）雖能減少衝突缺失，但會增加硬體複雜度與查找延遲。若並行比對能力不足，查找效能將大幅下降，甚至不如直接映射快取。 

關鍵點分析：
* Set Associative 的原理：資料映射到特定 Set，但可在該 Set 內任意 Way 存放，這減少了衝突。
* 硬體並行度限制：查找時，Set 內所有 Way 的 Tag 必須同時比對。如果一個 4-way Cache 只有 2 個比較器，就無法在一個週期內完成查找，導致並行度不足。
* 效能與功耗取捨：增加 Way 數雖可降低衝突，但硬體實現成本（比較器數量、多工器複雜度）會上升，若無法優化並行比對能力，將成為效能瓶頸。 


解決方案通常是在高相聯度與硬體查找速度（Latency）之間尋找平衡，例如使用更高速的比較器或採用更優化的分組策略。

# [Bus (computing)](https://en.wikipedia.org/wiki/Bus_(computing))
In computer architecture, a bus (historically also called a data highway[1] or databus) is a communication system that transfers data between components inside a computer or between computers.[2] It encompasses both hardware (e.g., wires, optical fiber) and software, including communication protocols.[3] At its core, a bus is a shared physical pathway, typically composed of wires, traces on a circuit board, or busbars, that allows multiple devices to communicate. To prevent conflicts and ensure orderly data exchange, buses rely on a communication protocol to manage which device can transmit data at a given time.

Buses are categorized based on their role, such as system buses (also known as internal buses, internal data buses, or memory buses) connecting the CPU and memory. Expansion buses, also called peripheral buses, extend the system to connect additional devices, including peripherals. Examples of widely used buses include PCI Express (PCIe) for high-speed internal connections and Universal Serial Bus (USB) for connecting external devices.


# [SPI](https://wiki.csie.ncku.edu.tw/embedded/SPI)
SPI 主控端（Master）連接的從屬裝置（Slave）數量沒有硬性上限，主要受限於 Master 晶片引腳數（需要為每個 Slave 準備一個 Chip Select/SS 引腳）和主板佈線能力，但性能會隨著 Slave 增加而下降，因為需要依次選中每個 Slave，且總線負載會增加。實際應用中，通常會透過增加 Chip Select (CS/SS) 訊號線來連接多個從屬元件，以實現與多個外設的通信。 

# [I2C](https://wiki.csie.ncku.edu.tw/embedded/I2C?revision=642383b3b702954e5f3a46beb8d894dd11086fe3)
An I2C master can theoretically connect to 128 devices (7-bit addressing) or 1024 devices (10-bit addressing), but the practical maximum is limited by total bus capacitance (around 400pF), which affects signal integrity, requiring careful selection of pull-up resistors and potentially slower speeds for many devices or longer distances. The actual limit depends heavily on the specific chips, bus speed (100kHz, 400kHz, 1MHz+), cable length, and pull-up resistor values, with bus extenders helping for longer distances. 


Further Reading: [spi i2c difference](https://www.google.com/search?q=spi+i2c+difference&oq=spi+i2c+difference&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIKCAEQABgFGBMYHjIKCAIQABgIGBMYHjIKCAMQABgIGBMYHjIKCAQQABgIGBMYHjIKCAUQABgIGBMYHjIKCAYQABgIGBMYHjIHCAcQABjvBTIKCAgQABiABBiiBDIKCAkQABiABBiiBNIBCDcxOTlqMGo3qAIIsAIB8QWr39-s2WLOIQ&sourceid=chrome&ie=UTF-8)

# Bit-Banging
using a group of flexible GPIO pins on a microcontroller or development board (like Raspberry Pi) to mimic standard communication buses (like I2C, SPI, or even parallel buses) through software bit-banging, allowing it to communicate with various peripherals that expect these protocols, often for simpler or custom interfaces. It involves configuring pins as outputs to send data and inputs to receive, sometimes requiring specific software drivers or careful pin management for different protocols like 1-Wire or PS/2, providing flexibility but potentially slower than dedicated hardware buses.


# GPIO

# PHY (Physical Layer)
PHY (Physical Layer) refers to the hardware component or circuitry that handles the actual transmission and reception of raw data bits over a physical medium (like copper wire, fiber optic, or airwaves), converting digital data into electrical, optical, or radio signals and vice back, bridging the digital world (MAC Layer) and the physical cable. It's crucial for basic connectivity, managing signal conditioning, encoding/decoding, and link training, ensuring stable data transfer. I.e., *Ethernet PHY*, *USB PHY*,

# MDIO
MDIO (Management Data Input/Output) is a 2-wire serial bus defined by IEEE 802.3 (Clause 22/45) for managing Ethernet Physical Layer (PHY) devices. It consists of a Management Data Clock (MDC) and data line (MDIO), enabling a Master (MAC) to read/write registers in up to 32 slave devices to control link speed, duplex mode, and power. 

*MDC (Clock)*: Driven by the MAC to the PHY to synchronize data.
*MDIO (Data)*: Bidirectional data line, allowing the MAC to write to and read from PHY registers.

Control bus.
[Management Data Input/Output](https://en.wikipedia.org/wiki/Management_Data_Input/Output)


# MII
Data bus.
[Media-independent interface](https://en.wikipedia.org/wiki/Media-independent_interface)

# DTS

# Level Trigger / Edge Trigger
[Interrupt](https://en.wikipedia.org/wiki/Interrupt#Triggering_methods)

# DRAM
## Double data rate (DDR)
[Double data rate](https://en.wikipedia.org/wiki/Double_data_rate)
> In computing, double data rate (DDR) describes a computer bus that transfers data on both the rising and falling edges of the clock signal and hence doubles the memory bandwidth by transferring data twice per clock cycle.[1][2] This is also known as *double pumped*, *dual-pumped*, and *double transition*.
> 
> The term toggle mode is used in the context of NAND flash memory.A comparison between `single data rate`, `double data rate`, and `quad data rate`. The dots are where data transfers take place, measured in millions of transfers per second (MT/s).



## Theoretical DDR bandwidth
Theoretical DRAM bandwidth is calculated by multiplying the transfer rate `(MT/s)` by the bus width `(bytes)` and the number of channels. The standard formula for peak theoretical bandwidth is:
> Transfer Rate (MT/s) x 8 bytes (64-bit) (memory bus width) x Number of Channels


This produces the maximum throughput in MB/s or GB/s, typically assuming 64-bit width per channel. 

For example:
$6000 (MT/s) \times 64 (Bits) \times 1 (channel) = 384000 (Bits/s) \approx 44 (GB/s)$

``` console
$ sudo dmidecode -t memory | grep -A16 'Memory Device' | grep 'Size'

        Size: No Module Installed
        Size: 16 GB
        Size: No Module Installed
        Size: 16 GB

$ sudo dmidecode -t memory | grep -A16 'Memory Device' | grep 'Speed'

        Speed: 6000 MT/s
        Speed: 6000 MT/s
        
$ sudo dmidecode -t memory | grep -A16 'Memory Device' | grep 'Data Width'
        Data Width: Unknown
        Data Width: 64 bits
        Data Width: Unknown
        Data Width: 64 bits
        
$ sudo dmidecode -t memory | grep Channel
```

# 32/64 Bit Architecture
The 32/64 Bit is meaning register size or bus width?. 32 bits address bus can address 2^{32} memory address; 64 bits address bus can address 2^{64} memory address.

The term “64-bit machine” is vague. Computers processors and systems have several features which may have sizes different from each other on the same machine, including:
* The width of most processor registers.
* The width of addresses.
* The width of the data bus.
* The width of the arithmetic logic units.

# Memory Module
## Dual In-line Memory Module (DIMM)
[DIMM](https://en.wikipedia.org/wiki/DIMM)

## Memory Read
資料流動過程 (Data Flow)
1. Core 發出讀取請求，L1 Cache 沒中 (Miss)。
2. 請求被掛載到一個空的 Line Full Buffer (LFB)。
3. LFB 詢問 L2/L3，若都沒中，請求轉發給 Integrated Memory Controller (IMC)。
3. IMC 從 RAM 搬回 64 Bytes 的資料片段。
4. LFB 接收碎片並湊齊一個完整 Line。
5. LFB 將資料寫入 L1 並通知 Core：資料準備好了！

# Core
## Core Bandwidth
## 單核心記憶體頻寬 (受限於延遲)
雖然 DDR5-6000 的總頻寬可達 48 GB/s，但單個核心通常無法跑滿這個數值。這是因為單核心受限於「未完成請求」(Concurrency) 的硬體緩衝區限制（Little's Law）。 

參數（典型值）：
* Memory Latency (DDR4)：around 15 ns
* Cache Line Size：64 Bytes
* Line Fill Buffer (LFB)（Little's Law）：約 10–12 個
* 公式：(Line Fill Buffer × Cache Line Size) / Memory Latency
* 計算：$(12 \times 64 Bytes) \div 15 ns \approx 47.6 GB/s$
 
Yes! It needs **two** core to saturate Memory bandwidth.

這是一個非常深刻的觀察！在計算記憶體頻寬或延遲時，我們似乎「忽略」了 CPU 的時脈（例如 5.0 GHz），這背後有幾個關鍵的原因：

**CPU 和記憶體運行在完全不同的時鐘域（Clock Domains）中。**

* CPU： 運行速度極快（週期約 0.2 ns）。
* 記憶體總線： 運行速度相對慢得多。
* 緩衝區的作用： 當 CPU 需要資料時，它會將請求丟入 LFB (Line Fill Buffers)。一旦請求離開了 CPU 核心進入記憶體控制器（IMC），它就進入了「等待物理傳輸」的階段。此時，CPU 的時脈再快，也無法加速電子在 PCB 板上的傳輸速度或 DRAM 電容的充放電。


# Chip V.S. Processor V.s Core
[Multi-core processor](https://en.wikipedia.org/wiki/Multi-core_processor)

One **System on a Chip (SoC)** be composed of Multiple **Processor** and **Memory**.
One **Processor (Processor Chip) (CPU)** be composed of Multiple **Core** and **L2 and L3 cache**.
**Core** is the Smallest unit of execution and be composed of **L1 cache**.

* **System on a Chip (SoC)**: Combines a processor (CPU), main memory (RAM), input/output controllers, and sometimes graphics (GPU) onto one chip.

* **Processor Chip (CPU)**: Primarily designed for logic and arithmetic, though they often contain small, fast "cache" memory on-die, rather than the large capacity "main" memory.

* **Traditional PC Architecture**: In many desktop computers, the CPU and main memory are physically separate chips on the motherboard, connected via a bus. 


$$
Chip > Processor > Core
$$

# Cache
## Cache Concurrency
managing simultaneous read/write access to cached data by multiple threads or processes.

Cache 硬體並行度不足主要發生在 Set Associative Cache 中，當 Way（路）數過多但缺乏足夠的比較器（Comparator）同時比對 Tag 時。高組相聯（High-way）雖能減少衝突缺失，但會增加硬體複雜度與查找延遲。若並行比對能力不足，查找效能將大幅下降，甚至不如直接映射快取。 

關鍵點分析：
* Set Associative 的原理：資料映射到特定 Set，但可在該 Set 內任意 Way 存放，這減少了衝突。
* 硬體並行度限制：查找時，Set 內所有 Way 的 Tag 必須同時比對。如果一個 4-way Cache 只有 2 個比較器，就無法在一個週期內完成查找，導致並行度不足。
* 效能與功耗取捨：增加 Way 數雖可降低衝突，但硬體實現成本（比較器數量、多工器複雜度）會上升，若無法優化並行比對能力，將成為效能瓶頸。 


解決方案通常是在高相聯度與硬體查找速度（Latency）之間尋找平衡，例如使用更高速的比較器或採用更優化的分組策略。

