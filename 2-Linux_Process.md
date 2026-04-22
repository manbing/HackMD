
```
.. SPDX-License-Identifier: GPL-2.0

==============================================
2. Explain your understanding of Linux process
==============================================

Process has independent resources (virtual address space). It has user address
space and kernel address space. It can run in user mode or kernel mode.

2a. What is the relation of interrupt, softirq, tasklet, workqueue, kthread, thread
-----------------------------------------------------------------------------------

When peripheral happens event, i.e., user press keyboard, peripheral will
send **Interrupt** to notify CPU. CPU receives the notification and
Interrupt ReQuest (IRQ) number. CPU calls the corresponding Interrupt
Service Routine (ISR), according to the interrupt request number.

In order to reduce interrupt latecy. Linux kernel separates ISR into two
parts, top half and bottom half.

Bottom half includes **softirq**, **tasklet** and **workqueue**:

* Softirq
	For efficiency, the same softirq can run on different core parallel.
	i.e., network packet receiving and transmitting. Therefore, it needs
	to consider synchronization problem.

* Tasklet
	For simple usage. the same tasklet only can run on one core at a same
	time. However, different tasklets can run on others core simultaneously.

* Workqueue
	For delay and sleep request. Workqueue is **kernel thread (kthread)**
	and running in process context, not atomic context. Like, Softirq and
	Tasklet.

Kthread is a task. Which has own kerenl address space and running in kerenl
mode only. **Thread** is also called as Light-Weight Process (LWP). It
shares resource (i.e., signal, address space and so on) of the specific
process with others threads.

2b. What the CPU scheduler schedule above process unit and what's different under preempt OS or RTOS
----------------------------------------------------------------------------------------------------

**Workqueue**, **kthread** and **thread** is schedulable in vanilla Linux
kernel. Because those are task, in process context.

In Real-Time Operating System (RTOS). RTOS will put interrupt handler into
thread (process context), and no longer run in interrupt context. Therefore,
it can schedule **interrupt**, **softirq**, **tasklet**, **workqueue**,
**ktherad** and **thread** in RTOS. This customization is for scheduler can
manage them more effectively and deterministically.

2c. How to protect critical section and how to improve critical section performance. (May need to consider user space, kernel space and mixed)
----------------------------------------------------------------------------------------------------------------------------------------------

How to protect critical section
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It can use spinlocks or mutexes to guard data in Linux kernel. Considering
different context, it can brief:

+---------------+-----------+-----------+-----------+
| Context		| User		| Softirq	| Hardirq	|
+---------------+-----------+-----------+-----------+
| User          | MLI()		|			|			|
+---------------+-----------+-----------+-----------+
| Softirq       | SLBH()	| SL()		|			|
+---------------+-----------+-----------+-----------+
| Hardirq		| MLI() 	| SLI()		| SLIS()	|
|				| DI()		|			|			|
+---------------+-----------+-----------+-----------+

Table:

+-------+-------------------------------+
| MLI	| mutex_lock_interruptible()	|
+-------+-------------------------------+
| SLBH	| spin_lock_bh()				|
+-------+-------------------------------+
| SLI	| spin_lock_irq()				|
+-------+-------------------------------+
| SL	| spin_lock()					|
+-------+-------------------------------+
| SLIS	| spin_lock_irqsave()			|
+-------+-------------------------------+
| DI	| disable_irq()					|
+-------+-------------------------------+

how to improve critical section performance
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If shared data to be read more than updated. In this usage scenario, it
can use lock-free read/write locking, Read Copy Update (RCU).

2d. (Bonus) How to consider cache line in your design
-----------------------------------------------------

Well Cache-aware design can improve system performance by the following
characteristic:

1. Decrease cache misses
   - Do data alignment.

2. Lower frequency of data loading
   - Take care spatial locality, temporal locality and loop ordering.

```