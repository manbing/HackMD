---
title: Computer Science Introduction
tags: [Computer science]

---

# Computer Science Introduction

## Memory hierarchy

* **Cache line** is the unit of data transfer between CPU and main memory. The cache line of your PC is most likely 64 bytes, meaning that the main memory is divided into blocks of 64 bytes, and whenever you request a byte, you are also fetching its cache line neighbours regardless whether your want it or not. Fetching a cache line is like grabbing a 6-pack.

* **Eviction policy** is the method for deciding which data to retain in the cache. In CPUs, it is controlled by hardware, not software. For simplicity, programmer can assume that least recently used (LRU) policy is used, which just evicts the item that hasn’t been used for the longest amount of time. This is like preferring beer with later expiration dates.


[Eytzinger Binary Search](https://algorithmica.org/en/eytzinger?fbclid=IwAR2IW7hER0jUWdEd_b6G3UU_goaAAFVpySO7bkMrGQ_Ecs0YodSg6NyYjy8)

## Multi-Programming
>In the early days of computing, CPU time was expensive, and peripherals were very slow. When the computer ran a program that needed access to a peripheral, the central processing unit (CPU) would have to stop executing program instructions while the peripheral processed the data. This was usually very inefficient.
>
>The first computer using a multiprogramming system was the British Leo III owned by J. Lyons and Co. During batch processing, several different programs were loaded in the computer memory, and the first one began to run. When the first program reached an instruction waiting for a peripheral, the context of this program was stored away, and the second program in memory was given a chance to run. The process continued until all programs finished running.
>
> The use of multiprogramming was enhanced by the arrival of **virtual memory** and **virtual machine** technology, which enabled individual programs to make use of memory and operating system resources as if other concurrently running programs were, for all practical purposes, nonexistent.
> 
> Multiprogramming gives no guarantee that a program will run in a timely manner. Indeed, the first program may very well run for hours without needing access to a peripheral. As there were no users waiting at an interactive terminal, this was no problem: users handed in a deck of punched cards to an operator, and came back a few hours later for printed results. Multiprogramming greatly reduced wait times when multiple batches were being processed.[4][5]


## Interrupt V.S. Preemoption
>When a process is **interrupted**. after interrupted handler is done, two things can happen:
>
>1. The same process will get the CPU again.
>2. A different process will get the CPU. The current process was **preempted**.
>
>So preemption will only happen after an interrupt, but an interrupt doesn't always cause preemption.

## 32/64 bits central central processing unit(CPU)
> depending on **address bus width of CPU**. 32 bits address bus can address 2^{32} memory address; 64 bits address bus can address 2^{64} memory address.

## 32/64 bits operation system(OS)
> 32 bits system expects it can access 2^{32} memory address; 64 bits system expects it can access 2^{64} memory address.


## Memory access and alignment
>An aligned memory access means that the pointer (as an integer) is a multiple of a type-specific value called the alignment.

[你所不知道的 C 語言：記憶體管理、對齊及硬體特性](https://hackmd.io/@sysprog/c-memory)

[Memory access and alignment](https://lwn.net/Articles/260832/)


## Microarchitecture
> In computer engineering, microarchitecture, also called ***computer organization*** and sometimes abbreviated as ***µarch*** or ***uarch***, is the way a given instruction set architecture (ISA) is implemented in a particular processor. A given ISA may be implemented with different microarchitectures; implementations may vary due to different goals of a given design or due to shifts in technology.
>
>Computer architecture is the combination of microarchitecture and instruction set architecture.

[Microarchitecture](https://en.wikipedia.org/wiki/Microarchitecture)

## CPU
### Register file
Swtch (kernel/swtch.S:3) saves only callee-saved registers; caller-saved registers are saved on the stack (if needed) by the calling C code.

#### Context Switch
CPU context switching has at least three different types
* Multitasking

* Iinterrupt(exception) handling
* User and kernel mode switching

#### Calling convention

### CPU mode
Intel: ring0, ring1 and ring3

RISC-V: Machine mode, kernel mode, and user mode


## Virtualization
### Containers V.S. Virtual Machines
`Virtual Machines`(VMs) include the guest operating system (OS) along with all the code for their applications and application dependencies that formerly ran on a single server or from a server pool. The size of VM images is generally measured in gigabytes. Multiple VMs can exist on a single physical server even if they are running on different operating systems. VMs abstract servers from the underlying hardware and typically persist throughout their useful life.

`Containers` share the host OS and include only the applications and their dependencies. The size of container images is generally measured in megabytes. Every container running on a single server shares the same underlying OS. Containers thus can spin up in milliseconds and are more efficient for ephemeral use cases where instances must be spun up and down with changes in demand.

[VMs vs Containers](https://www.vmware.com/topics/vms-vs-containers)


## Physical Addresses (Memory Map)
Real hardware places ==RAM== and devices at unpredictable ==physical addresses==.

## [Coroutine](https://en.wikipedia.org/wiki/Coroutine)
Coroutines are computer program components that allow execution to be suspended and resumed, generalizing subroutines for cooperative multitasking. Coroutines are well-suited for implementing familiar program components such as cooperative tasks, exceptions, event loops, iterators, infinite lists and pipes.

They have been described as "functions whose execution you can pause".[1]

Melvin Conway coined the term coroutine in 1958 when he applied it to the construction of an assembly program.[2] The first published explanation of the coroutine appeared later, in 1963.[3]
# Computer Science Introduction

## Memory hierarchy

* **Cache line** is the unit of data transfer between CPU and main memory. The cache line of your PC is most likely 64 bytes, meaning that the main memory is divided into blocks of 64 bytes, and whenever you request a byte, you are also fetching its cache line neighbours regardless whether your want it or not. Fetching a cache line is like grabbing a 6-pack.

* **Eviction policy** is the method for deciding which data to retain in the cache. In CPUs, it is controlled by hardware, not software. For simplicity, programmer can assume that least recently used (LRU) policy is used, which just evicts the item that hasn’t been used for the longest amount of time. This is like preferring beer with later expiration dates.


[Eytzinger Binary Search](https://algorithmica.org/en/eytzinger?fbclid=IwAR2IW7hER0jUWdEd_b6G3UU_goaAAFVpySO7bkMrGQ_Ecs0YodSg6NyYjy8)

## Multi-Programming
>In the early days of computing, CPU time was expensive, and peripherals were very slow. When the computer ran a program that needed access to a peripheral, the central processing unit (CPU) would have to stop executing program instructions while the peripheral processed the data. This was usually very inefficient.
>
>The first computer using a multiprogramming system was the British Leo III owned by J. Lyons and Co. During batch processing, several different programs were loaded in the computer memory, and the first one began to run. When the first program reached an instruction waiting for a peripheral, the context of this program was stored away, and the second program in memory was given a chance to run. The process continued until all programs finished running.
>
> The use of multiprogramming was enhanced by the arrival of **virtual memory** and **virtual machine** technology, which enabled individual programs to make use of memory and operating system resources as if other concurrently running programs were, for all practical purposes, nonexistent.
> 
> Multiprogramming gives no guarantee that a program will run in a timely manner. Indeed, the first program may very well run for hours without needing access to a peripheral. As there were no users waiting at an interactive terminal, this was no problem: users handed in a deck of punched cards to an operator, and came back a few hours later for printed results. Multiprogramming greatly reduced wait times when multiple batches were being processed.[4][5]


## Interrupt V.S. Preemoption
>When a process is **interrupted**. after interrupted handler is done, two things can happen:
>
>1. The same process will get the CPU again.
>2. A different process will get the CPU. The current process was **preempted**.
>
>So preemption will only happen after an interrupt, but an interrupt doesn't always cause preemption.

## 32/64 bits central central processing unit(CPU)
> depending on **address bus width of CPU**. 32 bits address bus can address 2^{32} memory address; 64 bits address bus can address 2^{64} memory address.

## 32/64 bits operation system(OS)
> 32 bits system expects it can access 2^{32} memory address; 64 bits system expects it can access 2^{64} memory address.


## Memory access and alignment
>An aligned memory access means that the pointer (as an integer) is a multiple of a type-specific value called the alignment.

[你所不知道的 C 語言：記憶體管理、對齊及硬體特性](https://hackmd.io/@sysprog/c-memory)

[Memory access and alignment](https://lwn.net/Articles/260832/)


## Microarchitecture
> In computer engineering, microarchitecture, also called ***computer organization*** and sometimes abbreviated as ***µarch*** or ***uarch***, is the way a given instruction set architecture (ISA) is implemented in a particular processor. A given ISA may be implemented with different microarchitectures; implementations may vary due to different goals of a given design or due to shifts in technology.
>
>Computer architecture is the combination of microarchitecture and instruction set architecture.

[Microarchitecture](https://en.wikipedia.org/wiki/Microarchitecture)

## CPU
### Register file
Swtch (kernel/swtch.S:3) saves only callee-saved registers; caller-saved registers are saved on the stack (if needed) by the calling C code.

#### Context Switch
CPU context switching has at least three different types
* Multitasking

* Iinterrupt(exception) handling
* User and kernel mode switching

#### Calling convention

### CPU mode
Intel: ring0, ring1 and ring3

RISC-V: Machine mode, kernel mode, and user mode


## Virtualization
### Containers V.S. Virtual Machines
`Virtual Machines`(VMs) include the guest operating system (OS) along with all the code for their applications and application dependencies that formerly ran on a single server or from a server pool. The size of VM images is generally measured in gigabytes. Multiple VMs can exist on a single physical server even if they are running on different operating systems. VMs abstract servers from the underlying hardware and typically persist throughout their useful life.

`Containers` share the host OS and include only the applications and their dependencies. The size of container images is generally measured in megabytes. Every container running on a single server shares the same underlying OS. Containers thus can spin up in milliseconds and are more efficient for ephemeral use cases where instances must be spun up and down with changes in demand.

[VMs vs Containers](https://www.vmware.com/topics/vms-vs-containers)


## Physical Addresses (Memory Map)
Real hardware places ==RAM== and devices at unpredictable ==physical addresses==.

## [Coroutine](https://en.wikipedia.org/wiki/Coroutine)
Coroutines are computer program components that allow execution to be suspended and resumed, generalizing subroutines for cooperative multitasking. Coroutines are well-suited for implementing familiar program components such as cooperative tasks, exceptions, event loops, iterators, infinite lists and pipes.

They have been described as "functions whose execution you can pause".[1]

Melvin Conway coined the term coroutine in 1958 when he applied it to the construction of an assembly program.[2] The first published explanation of the coroutine appeared later, in 1963.[3]
