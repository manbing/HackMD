---
title: Interrupt
tags: [Linux]

---

# Interrupt
###### tags: `Linux`

对于中断处理而言，linux将其分成了两个部分，一个叫做中断handler（top half），是全程关闭中断的，另外一部分是deferable task（bottom half），属于不那么紧急需要处理的事情。在执行bottom half的时候，是开中断的。有多种bottom half的机制，例如：softirq、tasklet、workqueue或是直接创建一个kernel thread来执行bottom half（这在旧的kernel驱动中常见，现在，一个理智的driver厂商是不会这么做的）。本文主要讨论softirq机制。由于tasklet是基于softirq的，因此本文也会提及tasklet，但主要是从需求层面考虑，不会涉及其具体的代码实现。

在普通的驱动中一般是不会用到softirq，但是由于驱动经常使用的tasklet是基于softirq的，因此，了解softirq机制有助于撰写更优雅的driver。softirq不能动态分配，都是静态定义的。内核已经定义了若干种softirq number，例如网络数据的收发、block设备的数据访问（数据量大，通信带宽高），timer的deferable task（时间方面要求高）。


## 为何有top half和bottom half

中断处理模块是任何OS中最重要的一个模块，对系统的性能会有直接的影响。想像一下：如果在通过U盘进行大量数据拷贝的时候，你按下一个key，需要半秒的时间才显示出来，这个场景是否让你崩溃？因此，对于那些复杂的、需要大量数据处理的硬件中断，我们不能让handler中处理完一切再恢复现场（handler是全程关闭中断的），而是仅仅在handler中处理一部分，具体包括：

（1）有实时性要求的

（2）和硬件相关的。例如ack中断，read HW FIFO to ram等

（3）如果是共享中断，那么获取硬件中断状态以便判断是否是本中断发生

除此之外，其他的内容都是放到bottom half中处理。在把中断处理过程划分成top half和bottom half之后，关中断的top half被瘦身，可以非常快速的执行完毕，大大减少了系统关中断的时间，提高了系统的性能。


interrupt context consist of hard IRQ context (top half) and soft IRQ context (bottom half).

| Type | Name | Context | local top half (irq) | local bottom half (bh) |preemption
| -------- | -------- | -------- | -------- | -------- | --------
| Top half     | Hardirq     | hard IRQ | disable | |
| Bottom half     | Softirq     | soft IRQ | enable
| Bottom half    | Tasklet     | soft IRQ     | enable
| Bottom half     | Workqueue     | process     | enable

## Top half
## Bottom half
workqueue和softirq、tasklet有本质的区别：workqueue运行在process context，而softirq和tasklet运行在interrupt context。因此，出现workqueue是不奇怪的，在有sleep需求的场景中，defering task必须延迟到kernel thread中执行，也就是说必须使用workqueue机制。softirq和tasklet是怎么回事呢？从本质上将，bottom half机制的设计有两方面的需求，一个是性能，一个是易用性。设计一个通用的bottom half机制来满足这两个需求非常的困难，因此，内核提供了softirq和tasklet两种机制。softirq更倾向于性能，而tasklet更倾向于易用性。

### Workqueue
在有sleep需求的场景中，defering task必须延迟到kernel thread中执行，也就是说必须使用workqueue机制。

### Softirq
为了性能，同一类型的softirq有可能在不同的CPU上并发执行，这给使用者带来了极大的痛苦，因为驱动工程师在撰写softirq的回调函数的时候要考虑重入，考虑并发，要引入同步机制。但是，为了性能，我们必须如此。

``` c
// include/linux/interrupt.h

enum
{
    HI_SOFTIRQ=0,
    TIMER_SOFTIRQ,
    NET_TX_SOFTIRQ,
    NET_RX_SOFTIRQ,
    BLOCK_SOFTIRQ,
    IRQ_POLL_SOFTIRQ,
    TASKLET_SOFTIRQ,
    SCHED_SOFTIRQ,
    HRTIMER_SOFTIRQ,
    RCU_SOFTIRQ,    /* Preferable RCU should always be the last softirq */

    NR_SOFTIRQS
};
```

### Tasklet
如果一个tasklet在processor A上被调度执行，那么它永远也不会同时在processor B上执行，也就是说，tasklet是串行执行的（注：不同的tasklet还是会并发的），不需要考虑重入的问题。

## Why it can not sleep in interrupt context

> 当然，linux的设计并非如此（其实在rt linux中已经有了这样的苗头，可以参考中断线程化的文章），中断handler以及bottom half（不包括workqueue）都是在interrupt context中执行。当然一提到context，各种资源还是要存在的，例如说内核栈、例如说memory space等，interrupt context虽然单薄，但是可以借尸还魂。当中断产生的那一个时刻，当前进程有幸成为interrupt context的壳，提供了内核栈，保存了hardware context，此外各种资源（例如mm_struct）也是借用当前进程的。本来呢interrupt context身轻如燕，没有依赖的task，调度器其实是不知道如何调度interrupt context的（它处理的都是task），在interrupt context借了一个外壳后，从理论上将，调度器是完全可以block该interrupt context执行，并将其他的task调入进入running状态。然而，block该interrupt context执行也就block其外壳task的执行，多么的不公平，多么的不确定，中断命中你，你就活该被schedule out，拥有正常思维的linux应该不会这么做的。 [5]

> Generally speaking, tasklets "borrow" the stack of whatever process happened to be running.
You cannot sleep in an interrupt handler because interrupts do not have a backing process context, and thus there is nothing to reschedule back into. In other words, interrupt handlers are not associated with a task, so there is nothing to "put to sleep" and (more importantly) "nothing to wake up". They must run atomically. 
Stepping back a bit, hardware interrupts come along and interrupt whatever task happened to be running. When the hardware IRQ stack gets back down to the task level again (i.e. all nested HW IRQs are processed, then it enabled interrupts and starts any queued tasklets. So tasklets are really still "interrupt" context, but with interrupts enabled.) [4]

The known blockable/sleepable API:

| API Name |
| -------- |
| kmalloc()     |
| netlink_broadcast()     |
| kobject_uevent_env()|


## 中断的打开和关闭
我们再来看看整个通用中断处理过程中的开关中断情况，开关中断有两种：
1. 开关local CPU的中断。对于UP，关闭CPU中断就关闭了一切，永远不会被抢占。对于SMP，实际上，没有关全局中断这一说，只能关闭local CPU（代码运行的那个CPU)。 e.x., local_disable_irq()
2. 控制interrupt controller，关闭某个IRQ number对应的中断。更准确的术语是mask或者unmask一个 IRQ。 e.x., disable_irq()

和硬件中断一样，软中断也可以disable，接口函数是local_bh_disable和local_bh_enable。


## Interrupt Latency

[Measuring Interrupt Latency](https://www.nxp.com/docs/en/application-note/AN12078.pdf)

> the delay between a hardware device generating an `interrupt request (IRQ)` and the Linux kernel starting the corresponding `interrupt service routine (ISR)`.
> 
>The interrupt latency is usually affected by a lot of factors.
For a narrow sense of the interrupt latency, there are these typical influence factors:
>* For most processor architectures, the processor usually completes the current instruction, which may be a multi-cycle instruction.
>
>* To save the current scene to restore the states when returning from the ISR, the processor pushes various necessary core registers (usually the program counter, flag registers, linker register, and so on) to the stack.
>
>* Some processor architectures need additional software statements to select the right ISR.
>
>* Time to fetch and decode the ISR instructions to fill the pipeline.
>
>* Most memory systems that store the code (such as flash) usually have wait states because the
memory system clock frequency is usually much slower than the CPU clock.
>
>* The interrupt may be preempted by other higher-priority interrupts anytime, including before the
first ISR instruction is executed.
>
> For a broad-sense definition of the interrupt latency, there are other additional factors:
>* The interrupt request signal must be synchronized to the CPU clock timing, which may take several cycles for the interrupt signal source to trigger the interrupt request.
>
>* If the interrupt request signal comes from outside of the processor device, the signal must be firstly synchronized to the bus/peripheral clock.
>
>* The RTOS may temporarily disable the interrupts when accessing critical resources. The latency is longer if the interrupt request asserts during the interrupt is disabled.
>
>* The response point may be defined as another event triggered by the ISR rather than the first instruction in the ISR, such as the response of an RTOS task which is blocked before and woken up by the interrupt. It may take many cycles for the software to complete the process before the
defined response point.



## Reference
[1] [linux kernel的中断子系统之（三）：IRQ number和中断描述符](http://www.wowotech.net/irq_subsystem/interrupt_descriptor.html)
[2] [linux kernel的中断子系统之（八）：softirq](http://www.wowotech.net/irq_subsystem/soft-irq.html)
[3] [why tasklet cant sleep](https://lists.kernelnewbies.org/pipermail/kernelnewbies/2011-November/003812.html)
[4] [Why kernel code/thread executing in interrupt context cannot sleep?](https://stackoverflow.com/questions/1053572/why-kernel-code-thread-executing-in-interrupt-context-cannot-sleep/1056710#1056710)
[5] [Concurrency Managed Workqueue之（一）：workqueue的基本概念](http://www.wowotech.net/irq_subsystem/workqueue.html)# Interrupt
###### tags: `Linux`

对于中断处理而言，linux将其分成了两个部分，一个叫做中断handler（top half），是全程关闭中断的，另外一部分是deferable task（bottom half），属于不那么紧急需要处理的事情。在执行bottom half的时候，是开中断的。有多种bottom half的机制，例如：softirq、tasklet、workqueue或是直接创建一个kernel thread来执行bottom half（这在旧的kernel驱动中常见，现在，一个理智的driver厂商是不会这么做的）。本文主要讨论softirq机制。由于tasklet是基于softirq的，因此本文也会提及tasklet，但主要是从需求层面考虑，不会涉及其具体的代码实现。

在普通的驱动中一般是不会用到softirq，但是由于驱动经常使用的tasklet是基于softirq的，因此，了解softirq机制有助于撰写更优雅的driver。softirq不能动态分配，都是静态定义的。内核已经定义了若干种softirq number，例如网络数据的收发、block设备的数据访问（数据量大，通信带宽高），timer的deferable task（时间方面要求高）。


## 为何有top half和bottom half

中断处理模块是任何OS中最重要的一个模块，对系统的性能会有直接的影响。想像一下：如果在通过U盘进行大量数据拷贝的时候，你按下一个key，需要半秒的时间才显示出来，这个场景是否让你崩溃？因此，对于那些复杂的、需要大量数据处理的硬件中断，我们不能让handler中处理完一切再恢复现场（handler是全程关闭中断的），而是仅仅在handler中处理一部分，具体包括：

（1）有实时性要求的

（2）和硬件相关的。例如ack中断，read HW FIFO to ram等

（3）如果是共享中断，那么获取硬件中断状态以便判断是否是本中断发生

除此之外，其他的内容都是放到bottom half中处理。在把中断处理过程划分成top half和bottom half之后，关中断的top half被瘦身，可以非常快速的执行完毕，大大减少了系统关中断的时间，提高了系统的性能。


interrupt context consist of hard IRQ context (top half) and soft IRQ context (bottom half).

| Type | Name | Context | local top half (irq) | local bottom half (bh) |preemption
| -------- | -------- | -------- | -------- | -------- | --------
| Top half     | Hardirq     | hard IRQ | disable | |
| Bottom half     | Softirq     | soft IRQ | enable
| Bottom half    | Tasklet     | soft IRQ     | enable
| Bottom half     | Workqueue     | process     | enable

## Top half
## Bottom half
workqueue和softirq、tasklet有本质的区别：workqueue运行在process context，而softirq和tasklet运行在interrupt context。因此，出现workqueue是不奇怪的，在有sleep需求的场景中，defering task必须延迟到kernel thread中执行，也就是说必须使用workqueue机制。softirq和tasklet是怎么回事呢？从本质上将，bottom half机制的设计有两方面的需求，一个是性能，一个是易用性。设计一个通用的bottom half机制来满足这两个需求非常的困难，因此，内核提供了softirq和tasklet两种机制。softirq更倾向于性能，而tasklet更倾向于易用性。

### Workqueue
在有sleep需求的场景中，defering task必须延迟到kernel thread中执行，也就是说必须使用workqueue机制。

### Softirq
为了性能，同一类型的softirq有可能在不同的CPU上并发执行，这给使用者带来了极大的痛苦，因为驱动工程师在撰写softirq的回调函数的时候要考虑重入，考虑并发，要引入同步机制。但是，为了性能，我们必须如此。

``` c
// include/linux/interrupt.h

enum
{
    HI_SOFTIRQ=0,
    TIMER_SOFTIRQ,
    NET_TX_SOFTIRQ,
    NET_RX_SOFTIRQ,
    BLOCK_SOFTIRQ,
    IRQ_POLL_SOFTIRQ,
    TASKLET_SOFTIRQ,
    SCHED_SOFTIRQ,
    HRTIMER_SOFTIRQ,
    RCU_SOFTIRQ,    /* Preferable RCU should always be the last softirq */

    NR_SOFTIRQS
};
```

### Tasklet
如果一个tasklet在processor A上被调度执行，那么它永远也不会同时在processor B上执行，也就是说，tasklet是串行执行的（注：不同的tasklet还是会并发的），不需要考虑重入的问题。

## Why it can not sleep in interrupt context

> 当然，linux的设计并非如此（其实在rt linux中已经有了这样的苗头，可以参考中断线程化的文章），中断handler以及bottom half（不包括workqueue）都是在interrupt context中执行。当然一提到context，各种资源还是要存在的，例如说内核栈、例如说memory space等，interrupt context虽然单薄，但是可以借尸还魂。当中断产生的那一个时刻，当前进程有幸成为interrupt context的壳，提供了内核栈，保存了hardware context，此外各种资源（例如mm_struct）也是借用当前进程的。本来呢interrupt context身轻如燕，没有依赖的task，调度器其实是不知道如何调度interrupt context的（它处理的都是task），在interrupt context借了一个外壳后，从理论上将，调度器是完全可以block该interrupt context执行，并将其他的task调入进入running状态。然而，block该interrupt context执行也就block其外壳task的执行，多么的不公平，多么的不确定，中断命中你，你就活该被schedule out，拥有正常思维的linux应该不会这么做的。 [5]

> Generally speaking, tasklets "borrow" the stack of whatever process happened to be running.
You cannot sleep in an interrupt handler because interrupts do not have a backing process context, and thus there is nothing to reschedule back into. In other words, interrupt handlers are not associated with a task, so there is nothing to "put to sleep" and (more importantly) "nothing to wake up". They must run atomically. 
Stepping back a bit, hardware interrupts come along and interrupt whatever task happened to be running. When the hardware IRQ stack gets back down to the task level again (i.e. all nested HW IRQs are processed, then it enabled interrupts and starts any queued tasklets. So tasklets are really still "interrupt" context, but with interrupts enabled.) [4]

The known blockable/sleepable API:

| API Name |
| -------- |
| kmalloc()     |
| netlink_broadcast()     |
| kobject_uevent_env()|


## 中断的打开和关闭
我们再来看看整个通用中断处理过程中的开关中断情况，开关中断有两种：
1. 开关local CPU的中断。对于UP，关闭CPU中断就关闭了一切，永远不会被抢占。对于SMP，实际上，没有关全局中断这一说，只能关闭local CPU（代码运行的那个CPU)。 e.x., local_disable_irq()
2. 控制interrupt controller，关闭某个IRQ number对应的中断。更准确的术语是mask或者unmask一个 IRQ。 e.x., disable_irq()

和硬件中断一样，软中断也可以disable，接口函数是local_bh_disable和local_bh_enable。


## Interrupt Latency

[Measuring Interrupt Latency](https://www.nxp.com/docs/en/application-note/AN12078.pdf)

> the delay between a hardware device generating an `interrupt request (IRQ)` and the Linux kernel starting the corresponding `interrupt service routine (ISR)`.
> 
>The interrupt latency is usually affected by a lot of factors.
For a narrow sense of the interrupt latency, there are these typical influence factors:
>* For most processor architectures, the processor usually completes the current instruction, which may be a multi-cycle instruction.
>
>* To save the current scene to restore the states when returning from the ISR, the processor pushes various necessary core registers (usually the program counter, flag registers, linker register, and so on) to the stack.
>
>* Some processor architectures need additional software statements to select the right ISR.
>
>* Time to fetch and decode the ISR instructions to fill the pipeline.
>
>* Most memory systems that store the code (such as flash) usually have wait states because the
memory system clock frequency is usually much slower than the CPU clock.
>
>* The interrupt may be preempted by other higher-priority interrupts anytime, including before the
first ISR instruction is executed.
>
> For a broad-sense definition of the interrupt latency, there are other additional factors:
>* The interrupt request signal must be synchronized to the CPU clock timing, which may take several cycles for the interrupt signal source to trigger the interrupt request.
>
>* If the interrupt request signal comes from outside of the processor device, the signal must be firstly synchronized to the bus/peripheral clock.
>
>* The RTOS may temporarily disable the interrupts when accessing critical resources. The latency is longer if the interrupt request asserts during the interrupt is disabled.
>
>* The response point may be defined as another event triggered by the ISR rather than the first instruction in the ISR, such as the response of an RTOS task which is blocked before and woken up by the interrupt. It may take many cycles for the software to complete the process before the
defined response point.



## Reference
[1] [linux kernel的中断子系统之（三）：IRQ number和中断描述符](http://www.wowotech.net/irq_subsystem/interrupt_descriptor.html)
[2] [linux kernel的中断子系统之（八）：softirq](http://www.wowotech.net/irq_subsystem/soft-irq.html)
[3] [why tasklet cant sleep](https://lists.kernelnewbies.org/pipermail/kernelnewbies/2011-November/003812.html)
[4] [Why kernel code/thread executing in interrupt context cannot sleep?](https://stackoverflow.com/questions/1053572/why-kernel-code-thread-executing-in-interrupt-context-cannot-sleep/1056710#1056710)
[5] [Concurrency Managed Workqueue之（一）：workqueue的基本概念](http://www.wowotech.net/irq_subsystem/workqueue.html)