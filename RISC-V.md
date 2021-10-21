# RISC-V
control and status registers (CSR)


From the Procedure Call Standard for the ***Arm Architecture***:
>Scratch register / temporary register A register used to hold an intermediate value during a calculation (usually, such values are not named in the program source and have a limited lifetime).

## calling convention
caller
ra, t0-t2, a0-a1, a2-a7, t3-t6
ft0-ft7, fa0-fa1, fa2-fa7, ft8 -ft11

callee
sp, fp, s1, s2-s11,
fs0-fs1, fs2-fs11

## riscv-privileged-v1.10

### 1.3 Privilege Levels

At any time, a RISC-V hardware thread (hart) is running at some privilege level encoded as a mode in one or more CSRs (control and status registers). Three RISC-V privilege levels are currently defined as shown in Table 1.1.

| Level | Encoding | Name             | Abbreviation |
| ----- | -------- | ---------------- | ------------ |
| 0     | 00       | User/Application | U            |
| 1     | 01       | Supervisor       | S            |
| 2     | 10       | Reserved         |              |
| 3     | 11       | Machine          | M            |

\begin{gather*}Table 1.1: RISC-V privilege levels.\end{gather*}

The machine level has the highest privileges and is the only mandatory privilege level for a RISC-V hardware platform. Code run in machine-mode (M-mode) is usually inherently trusted, as it has low-level access to the machine implementation. M-mode can be used to manage secure execution environments on RISC-V. User-mode (U-mode) and supervisor-mode (S-mode) are intended for conventional application and operating system usage respectively.

### 1.4 Debug Mode


### 4.1 Supervisor CSRs

#### 4.1.1 Supervisor Status Register (sstatus)
The SPP bit indicates the privilege level at which a hart was executing before entering supervisor mode. When a trap is taken, SPP is set to 0 if the trap originated from user mode, or 1 otherwise. When an SRET instruction is executed to return from the trap handler, the privilege level is set to user mode if the SPP bit is 0, or supervisor mode if the SPP bit is 1; SPP is then set to 0.


## sifive-interrupt-cookbook-v1p2

#### 3.12 CLINT Direct Mode
Direct mode means all interrupts and exceptions trap to the same handler, and there is no vector table implemented. It is software’s responsibility to execute code to figure out which interrupt occurred. The software handler in direct mode should first read ***mcause.interrupt*** to determine if an interrupt or exception occurred, then decide what to do based on ***mcause.code*** value which contains the respective interrupt or exception code.

##### 1.2.1 Local Interrupt Controllers
Both the CLINT and CLIC integrate registers ***mtime*** and ***mtimecmp*** to configure timer interrupts, and ***msip*** to trigger software interrupts. Additionally, both the CLINT and the CLIC run at the core clock frequency.

### 5 Privilege Levels
#### 5.1 Priorities for Supervisor & Machine Mode Interrupts

| Number of Levels | Supported Modes | Intended Usage |
| ---------------- | --------------- | -------------- |
| 1                | M               | Simple Embedded Systems |
| 2                | M, U            | Secure Embedded Systems |
| 3                | M, S, U         | Systems running Unix-like operating systems |


#### 5.2 Context Switch Overhead
Each privilege level has its own interrupt configuration registers, or a subset of bits in the respective machine mode only registers. Additionally, there is save/restore overhead to support a context switch to a different privilege level. Context switches include the following:
* 31 User registers, x1 - x31 (x0 is hardwired to 0)
* Program Counter (pc)
* 32 Floating point registers
* Floating point Control and Status Register (fcsr)

Prior to initiating a context switch, these registers should be saved on the stack. Likewise, prior to returning from a different privilege level, they are required to be restored from the stack.

A Context switch is initiated through the ECALL instruction which results in a environment-call- from-x-mode exception, where x is either M, S, or U, depending on the current privilege level. Return from an ECALL instruction occurs by executing the respective xRET instruction, where again, x is M, S, or U. The xRET instruction can be used in privilege mode x or higher. For more information on context switches, refer to The RISC-V Instruction Set Manual Volume II: Privi- leged Architecture Privileged Architecture Version 1.10

## Reference
riscv-privileged-v1.10.pdf
riscv-calling.pdf
riscv-spec-v2.2.pdf
sifive-interrupt-cookbook-v1p2.pdf
