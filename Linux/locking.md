# Locking (Synchronization)
###### tags: `Linux`

Can not sleep in spinlock is not precisely
---
In fact, sytem can not sleep in atomic context.
Like, Interruput context, holding spinlock and so one. 


## Spinlock
busy waiting


| API | preempt_disable() | local_irq_disable() | couple API |
| -------- | -------- | -------- | -------- |
| spin_lock() |    y  |   n   | spin_unlock() |
| spin_lock_bh() |   y   |   n   | spin_unlock_bh() |
| spin_lock_irq() |   y   |   y   | spin_unlock_irq() |
| spin_lock_irqsave() |   y   |   y   | spin_unlock_irqrestore() |


## Mutex
sleep waiting

| API  | couple API |
| -------- | -------- |
| mutex_lock() | mutex_unlock() |
| rt_mutex_lock() | rt_mutex_unlock() |

## Semaphore
sleep waiting

| API  | couple API |
| -------- | -------- |
| up() | down() |

## Context

### Interrupt
using Spinlock

### process context
using Spinlock, Mutex or Semaphore