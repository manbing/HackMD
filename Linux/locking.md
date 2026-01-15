---
title: Synchronization Primitive (Locking )
tags: [Linux]

---

# Locking (Synchronization)
###### tags: `Linux`

Can not sleep in spinlock is not precisely
---
In fact, sytem can not sleep in atomic context.
Like, Interruput context, holding spinlock and so one. 


## Spinlock
Busy waiting.

Lock:
Contender keep checking if lock is unlocker or not.

Unlock:
The contender which firstly get lock be owner.


| API | preempt_disable() | local_irq_disable() | couple API |
| -------- | -------- | -------- | -------- |
| spin_lock() |    y  |   n   | spin_unlock() |
| spin_lock_bh() |   y   |   n   | spin_unlock_bh() |
| spin_lock_irq() |   y   |   y   | spin_unlock_irq() |
| spin_lock_irqsave() |   y   |   y   | spin_unlock_irqrestore() |


## Mutex
Sleep waiting. Because it is sleep waiting, it can do notification and wakup process.

Lock:
Contender join <mark>the general wait queue</mark> and relinquish CPU and sleep.

Unlock:
The owner wak up the first contender in <mark>the general wait queue</mark> to be next owner.

| API  | couple API |
| -------- | -------- |
| mutex_lock() | mutex_unlock() |
| rt_mutex_lock() | rt_mutex_unlock() |

### Condition Variable

Lock:
Contender join <mark>the specific wait queue</mark> and relinquish CPU and sleep.



Unlock:
The owner wak up the first contender <mark>the specific wait queue</mark> to be next owner.

I.e., Ring buffer
``` c
void queue_put(queue_t *q, uint8_t *buffer, size_t size)
{
    pthread_mutex_lock(&q->lock);

    while ((q->tail + sizeof(size)) % q->size == q->head)
        pthread_cond_wait(&q->writeable, &q->lock);

    memcpy(&q->buffer[q->tail], buffer, sizeof(size_t));
     // printf("put : %ld\n", *(size_t *) buffer);
    q->tail += size;
    q->tail %= q->size;

    pthread_cond_signal(&q->readable);
    pthread_mutex_unlock(&q->lock);
}
```

``` c
size_t queue_get(queue_t *q, uint8_t *buffer, size_t max)
{
    pthread_mutex_lock(&q->lock);

    while ((q->tail - q->head) == 0)
        pthread_cond_wait(&q->readable, &q->lock);

    memcpy(buffer, &q->buffer[q->head], sizeof(size_t));
    // printf("get : %ld\n", *(size_t *) buffer);
    q->head += max;
    q->head %= q->size;

    pthread_cond_signal(&q->writeable);
    pthread_mutex_unlock(&q->lock);

    return max;
}
```


## Semaphore
Sleep waiting. It does not have owner concept. Process A wait \(P) a semaphore, Process B can signal (V) the same semaphore, even it is binaray semaphore.

[Semantics and implementation](https://en.wikipedia.org/wiki/Semaphore_(programming)#Semantics_and_implementation)

| API  | couple API |
| -------- | -------- |
| up() | down() |


## Atomic Operation

## Context

### Interrupt
using Spinlock

### process context
using Spinlock, Mutex or Semaphore

## Debug
KCSAN
# Locking (Synchronization)
###### tags: `Linux`

Can not sleep in spinlock is not precisely
---
In fact, sytem can not sleep in atomic context.
Like, Interruput context, holding spinlock and so one. 


## Spinlock
Busy waiting.

Lock:
Contender keep checking if lock is unlocker or not.

Unlock:
The contender which firstly get lock be owner.


| API | preempt_disable() | local_irq_disable() | couple API |
| -------- | -------- | -------- | -------- |
| spin_lock() |    y  |   n   | spin_unlock() |
| spin_lock_bh() |   y   |   n   | spin_unlock_bh() |
| spin_lock_irq() |   y   |   y   | spin_unlock_irq() |
| spin_lock_irqsave() |   y   |   y   | spin_unlock_irqrestore() |


## Mutex
Sleep waiting. Because it is sleep waiting, it can do notification and wakup process.

Lock:
Contender join <mark>the general wait queue</mark> and relinquish CPU and sleep.

Unlock:
The owner wak up the first contender in <mark>the general wait queue</mark> to be next owner.

| API  | couple API |
| -------- | -------- |
| mutex_lock() | mutex_unlock() |
| rt_mutex_lock() | rt_mutex_unlock() |

### Condition Variable

Lock:
Contender join <mark>the specific wait queue</mark> and relinquish CPU and sleep.



Unlock:
The owner wak up the first contender <mark>the specific wait queue</mark> to be next owner.

I.e., Ring buffer
``` c
void queue_put(queue_t *q, uint8_t *buffer, size_t size)
{
    pthread_mutex_lock(&q->lock);

    while ((q->tail + sizeof(size)) % q->size == q->head)
        pthread_cond_wait(&q->writeable, &q->lock);

    memcpy(&q->buffer[q->tail], buffer, sizeof(size_t));
     // printf("put : %ld\n", *(size_t *) buffer);
    q->tail += size;
    q->tail %= q->size;

    pthread_cond_signal(&q->readable);
    pthread_mutex_unlock(&q->lock);
}
```

``` c
size_t queue_get(queue_t *q, uint8_t *buffer, size_t max)
{
    pthread_mutex_lock(&q->lock);

    while ((q->tail - q->head) == 0)
        pthread_cond_wait(&q->readable, &q->lock);

    memcpy(buffer, &q->buffer[q->head], sizeof(size_t));
    // printf("get : %ld\n", *(size_t *) buffer);
    q->head += max;
    q->head %= q->size;

    pthread_cond_signal(&q->writeable);
    pthread_mutex_unlock(&q->lock);

    return max;
}
```


## Semaphore
Sleep waiting. It does not have owner concept. Process A wait \(P) a semaphore, Process B can signal (V) the same semaphore, even it is binaray semaphore.

[Semantics and implementation](https://en.wikipedia.org/wiki/Semaphore_(programming)#Semantics_and_implementation)

| API  | couple API |
| -------- | -------- |
| up() | down() |


## Atomic Operation

## Context

### Interrupt
using Spinlock

### process context
using Spinlock, Mutex or Semaphore

## Debug
KCSAN
