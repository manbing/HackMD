Hardware design supports two complementary methods of performing input/output (I/O) between the central processing unit (CPU) and peripheral devices in a comuputer (often mediating access via chipset).
* Memory-mapped I/O (MMIO)
* Port-mapped I/O (PMIO)

Memory-mapped I/O is mainstream.

# Memory-mapped I/O (MMIO)
**Memory-mapped I/O** uses the same address space to address both main memory and I/O devices. The memory and registers of the I/O devices are mapped to (associated with) address values, so a memory address may refer to either a portion of physical RAM or to memory and registers of the I/O device. Thus, the CPU instructions used to access the memory (e.g. MOV ...) can also be used for accessing devices. Each I/O device either monitors the CPU's address bus and responds to any CPU access of an address assigned to that device, connecting the system bus to the desired device's hardware register, or uses a dedicated bus.

# Physical Memory
**physical memory address space** (Physical memory). It is not the physical random-access memory (RAM).
![image](https://hackmd.io/_uploads/BJ-96vJEee.png)

physical memory consists of both physical RAM and I/O devices. The memory and registers of the I/O devices are mapped to (associated with) address values, so a memory address may refer to either a portion of physical RAM or to memory and registers of the I/O device.

it can be retrieved via `/proc/iomem`.

# Virtual Memory
**Virtual memory address space** (Virtual Memory). For efficiency reasons, the virtual address space is divided into **user space** and **kernel space**. For the same reason, the kernel space contains a memory mapped zone, called lowmem, which is contiguously mapped in physical memory, starting from the lowest possible physical address (usually 0). The virtual address where lowmem is mapped is defined by `PAGE_OFFSET`.

The basic unit for virtual memory management is a page, which size is usually 4K, but it can be up to 64K on some platforms. Whenever we work with virtual memory we work with two types of addresses: virtual address and physical address. <mark>All CPU access (including from kernel space) uses virtual addresses that are translated by the MMU into physical addresses with the help of page tables</mark>.

## User space

## Kernel space

## Process Memory Layout
The 4 GB address space in 32 bit x86 Linux is usually split into different sections for every process on the system:
* 0GB-1GB ==User space== - Used for text, code and `brk/sbrk` allocations. `malloc` uses `brk` for small chunks.
* 1GB-3GB ==User space== - Used for shared libraries, shared memory, and the stack. Shared memory and `malloc` use `mmap`. `malloc` uses `mmap` for large chunks.
* 3GB-4GB ==Kernel Space== - Used by and for the kernel itself

The split between userspace and kernelspace is set by the kernel parameter `PAGE_OFFSET` which is usually `0xc0000000` (3GB).
The address mappings of processes can be checked by viewing the proc file `/proc/<pid>/maps` where pid stands for the process ID.





# Reference
[Linux Memory Layout](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/5/html/tuning_and_optimizing_red_hat_enterprise_linux_for_oracle_9i_and_10g_databases/sect-oracle_9i_and_10g_tuning_guide-growing_the_oracle_sga_to_2.7_gb_in_x86_red_hat_enterprise_linux_2.1_without_vlm-linux_memory_layout#sect-Oracle_9i_and_10g_Tuning_Guide-Growing_the_Oracle_SGA_to_2.7_GB_in_x86_Red_Hat_Enterprise_Linux_2.1_Without_VLM-Linux_Memory_Layout)

[Memory-mapped I/O and port-mapped I/O](https://en.wikipedia.org/wiki/Memory-mapped_I/O_and_port-mapped_I/O)

[Memory mapping](https://linux-kernel-labs.github.io/refs/heads/master/labs/memory_mapping.html)

[Linux 核心的 `/dev/mem` 裝置](https://hackmd.io/@sysprog/linux-mem-device#Linux-%E6%A0%B8%E5%BF%83%E7%9A%84-devmem-%E8%A3%9D%E7%BD%AE)