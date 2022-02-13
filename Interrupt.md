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

## Top half
## Bottom half
workqueue和softirq、tasklet有本质的区别：workqueue运行在process context，而softirq和tasklet运行在interrupt context。

### Workqueue
在有sleep需求的场景中，defering task必须延迟到kernel thread中执行，也就是说必须使用workqueue机制。

softirq和tasklet是怎么回事呢？从本质上将，bottom half机制的设计有两方面的需求，一个是性能，一个是易用性。设计一个通用的bottom half机制来满足这两个需求非常的困难，因此，内核提供了softirq和tasklet两种机制。softirq更倾向于性能，而tasklet更倾向于易用性。

### Softirq & Tasklet
Generally speaking, tasklets "borrow" the stack of whatever process happened to be running.

You cannot sleep in an interrupt handler because interrupts do not have a backing process context, and thus there is nothing to reschedule back into. In other words, interrupt handlers are not associated with a task, so there is nothing to "put to sleep" and (more importantly) "nothing to wake up". They must run atomically.

Stepping back a bit, hardware interrupts come along and interrupt whatever task happened to be running. When the hardware IRQ stack gets back down to the task level again (i.e. all nested HW IRQs are processed, then it enabled interrupts and starts any queued tasklets. So tasklets are really still "interrupt" context, but with interrupts enabled.)



#### Softirq
为了性能，同一类型的softirq有可能在不同的CPU上并发执行，这给使用者带来了极大的痛苦，因为驱动工程师在撰写softirq的回调函数的时候要考虑重入，考虑并发，要引入同步机制。但是，为了性能，我们必须如此。

#### Tasklet
如果一个tasklet在processor A上被调度执行，那么它永远也不会同时在processor B上执行，也就是说，tasklet是串行执行的（注：不同的tasklet还是会并发的），不需要考虑重入的问题。


## 中断的打开和关闭
我们再来看看整个通用中断处理过程中的开关中断情况，开关中断有两种：
1. 开关local CPU的中断。对于UP，关闭CPU中断就关闭了一切，永远不会被抢占。对于SMP，实际上，没有关全局中断这一说，只能关闭local CPU（代码运行的那个CPU)。 e.x., local_disable_irq()
2. 控制interrupt controller，关闭某个IRQ number对应的中断。更准确的术语是mask或者unmask一个 IRQ。 e.x., disable_irq()


## Reference
[linux kernel的中断子系统之（三）：IRQ number和中断描述符](http://www.wowotech.net/irq_subsystem/interrupt_descriptor.html)
[linux kernel的中断子系统之（八）：softirq](http://www.wowotech.net/irq_subsystem/soft-irq.html)
[why tasklet cant sleep](https://lists.kernelnewbies.org/pipermail/kernelnewbies/2011-November/003812.html)
[Why kernel code/thread executing in interrupt context cannot sleep?](https://stackoverflow.com/questions/1053572/why-kernel-code-thread-executing-in-interrupt-context-cannot-sleep/1056710#1056710)