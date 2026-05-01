
[coding-style](https://docs.kernel.org/process/coding-style.html)
[clang-format](https://docs.kernel.org/dev-tools/clang-format.html)


# Kerenl Document
``` c
/**
 *  netif_receive_skb - process receive buffer from network
 *  @skb: buffer to process
 *
 *  netif_receive_skb() is the main receive data processing function.
 *  It always succeeds. The buffer may be dropped during processing
 *  for congestion control or by the protocol layers.
 *
 *  This function may only be called from softirq context and interrupts
 *  should be enabled.
 *
 *  Return values (usually ignored):
 *  NET_RX_SUCCESS: no congestion
 *  NET_RX_DROP: packet was dropped
 */
```

# Comment
``` c
    /* Note : we use a relaxed variant of napi_schedule_prep() not setting
     * NAPI_STATE_MISSED, since we do not react to a device IRQ.
     */

/* counter for the number of times this device was added to us */
```

# C

Header gurad:
``` c
#ifndef __NET_DEVLINK_H__
#define __NET_DEVLINK_H__

#endif /* !__NET_DEVLINK_H__ */
```

Type casting:
``` c

struct section *s = (struct section *s)e;

```

Declare:
``` c
char name[128] = {0};
```


``` c
return ret < 0 ? ret : 1;
```

Define:
``` c
struct dummy {
    
};
```

``` c
if () {
    
} else if () {
    
} else {
    
}
```

# Tool

* Check conding style
``` console
$ ./scripts/checkpatch.pl --no-tree -f --max-line-length=95 *.[ch]
```

``` console
# Make sure your working directory is clean!
$ clang-format -i kernel/*.[ch]
```

* check indent
``` console
$ indent -linux --line-length95 *.[chsS]
```
