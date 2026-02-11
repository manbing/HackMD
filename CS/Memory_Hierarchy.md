---
title: Memory
tags: [Computer Architecture]

---


# Memory Hierarchy
![image](https://hackmd.io/_uploads/SJlLyvtgbe.png)



* Cache (L1, L2, L3)
managed unit,[ Cache Line](https://en.wikipedia.org/wiki/CPU_cache#Cache_entry_structure). Size is 32, 64, 128 Bytes.

* Meomory (RAM)
managed unit, [Page](https://en.wikipedia.org/wiki/Memory_paging). Size is 4, 8 KiloByts.


# Memory Bandwidth

Bandwidth computation and nomenclature

The nomenclature differs across memory technologies, but for commodity DDR SDRAM, DDR2 SDRAM, and DDR3 SDRAM memory, the total bandwidth is the product of:

* **Base DRAM clock frequency**
Number of data transfers per clock: Two, in the case of "double data rate" (DDR, DDR2, DDR3, DDR4) memory.
* **Memory bus (interface) width:** Each DDR, DDR2, or DDR3 memory interface is 64 bits wide. Those 64 bits are sometimes referred to as a "line."
* **Number of interfaces:** Modern personal computers typically use two memory interfaces (dual-channel mode) for an effective 128-bit bus width.

For example, a computer with dual-channel memory and one DDR2-800 module per channel running at 400 MHz would have a theoretical maximum memory bandwidth of:
> 400,000,000 clocks per second × 2 lines per clock × 64 bits per line × 2 interfaces =
102,400,000,000 (102.4 billion) bits per second (in bytes, 12,800 MB/s or 12.8 GB/s)
This theoretical maximum memory bandwidth is referred to as the "burst rate," which may not be sustainable.

# Memory Hierarchy
![image](https://hackmd.io/_uploads/SJlLyvtgbe.png)



* Cache (L1, L2, L3)
managed unit,[ Cache Line](https://en.wikipedia.org/wiki/CPU_cache#Cache_entry_structure). Size is 32, 64, 128 Bytes.

* Meomory (RAM)
managed unit, [Page](https://en.wikipedia.org/wiki/Memory_paging). Size is 4, 8 KiloByts.


# Memory Bandwidth

Bandwidth computation and nomenclature

The nomenclature differs across memory technologies, but for commodity DDR SDRAM, DDR2 SDRAM, and DDR3 SDRAM memory, the total bandwidth is the product of:

* **Base DRAM clock frequency**
Number of data transfers per clock: Two, in the case of "double data rate" (DDR, DDR2, DDR3, DDR4) memory.
* **Memory bus (interface) width:** Each DDR, DDR2, or DDR3 memory interface is 64 bits wide. Those 64 bits are sometimes referred to as a "line."
* **Number of interfaces:** Modern personal computers typically use two memory interfaces (dual-channel mode) for an effective 128-bit bus width.

For example, a computer with dual-channel memory and one DDR2-800 module per channel running at 400 MHz would have a theoretical maximum memory bandwidth of:
> 400,000,000 clocks per second × 2 lines per clock × 64 bits per line × 2 interfaces =
102,400,000,000 (102.4 billion) bits per second (in bytes, 12,800 MB/s or 12.8 GB/s)
This theoretical maximum memory bandwidth is referred to as the "burst rate," which may not be sustainable.
