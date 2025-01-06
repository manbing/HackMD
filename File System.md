# File Systems
###### tags: `Linux`


## procfs [(5)](https://man7.org/linux/man-pages/man5/proc.5.html)
```
/proc/cpuinfo
/proc/kmsg
/proc/interrupts
/proc/mounts
/proc/timer_stats
/proc/sched_debug
/proc/timer_list
/proc/diskdump
/proc/slabinfo
/proc/kallsyms
/proc/partitions
/proc/kcore
/proc/sys/kernel/printk

/proc/cmdline
> kernel command line

/proc/iomem
```

```
/proc/<pid>/cmdline
/proc/<pid>/status
/proc/<pid>/maps
/proc/<pid>/coredump_filter
/proc/<pid>/oom_score
/proc/<pid>/oom_adj
/proc/<pid>/stat
/proc/<pid>/stack
```
  ```  
/proc/net/arp
/proc/net/vlan/config
/proc/net/bonding/config
/proc/net/igmp
```
```
/proc/sys/kernel/core_pattern
/proc/sys/net/core/rmem_max(SO_RCVBUF)
/proc/sys/net/core/wmem_max(SO_SNDBUF)
/proc/sys/vm/oom_dump_tasks
```

```
echo "value" > /proc/sysrq-trigger
/proc/sys/kernel/sysrq
```
    
    
### memory manage
>cat /proc/meminfo
    
#### overcommit & oom killer
```
/proc/sys/vm/overcommit_memory
/proc/<pid>/oom_adj
/proc/<pid>/oom_score
```

## sysfs
```
/sys/devices/system/cpu/cpu<N>/cache/index2/shared_cpu_list
/sys/devices/system/cpu/cpu<N>/cpufreq/
/sys/kernel/mm/ksm/
/sys/power/stat
/sys/bus/usb/devices/<usb id>/power/
/sys/module/usbcore/parameters/
    
/sys/kernel/kexec_crash_loaded
/sys/kernel/kexec_crash_size
/sys/class/net/br3/brif/<if name>/multicast_router
/sys/class/net/br0/brif/eth0.3/port_no
``` 
    
## debugfs
mount -t debugfs debugfs /sys/kernel/debug

/sys/kernel/debug/kmemleak
> echo scan > /sys/kernel/debug/kmemleak
echo clear > /sys/kernel/debug/kmemleak
cat /sys/kernel/debug/kmemleak
echo scan=off > /sys/kernel/debug/kmemleak
echo off > /sys/kernel/debug/kmemleak

### ftrace
```
/sys/kernel/debug/tracing/kprobe_events
/sys/kernel/debug/tracing/kprobe_profile
/sys/kernel/debug/tracing/trace
/sys/kernel/debug/tracing/trace_pipe
/sys/kernel/debug/tracing/trace_options
/sys/kernel/debug/tracing/events/kprobes/enable
/sys/kernel/debug/tracing/events/kprobes/<name>/enable
/sys/kernel/debug/tracing/events/kprobes/<name>/filter
```



## bpf
    
## dev
> /dev/watchdog

## else
cat /etc/shells
/dev/random