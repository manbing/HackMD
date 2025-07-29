---
title: Filesystem Hierarchy Standard
tags: [Linux]

---

# File System Hierarchy Standard
The [Filesystem Hierarchy Standard](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard) (FHS) is a reference describing the conventions used for the layout of Unix-like systems. It has been made popular by its use in Linux distributions, but it is used by other Unix-like systems as well. It is maintained by the Linux Foundation. The latest version is 3.0, released on 3 June 2015.


## [procfs](https://man7.org/linux/man-pages/man5/proc.5.html)
``` console
$ /proc/cpuinfo
$ /proc/kmsg
$ /proc/mounts
$ /proc/timer_stats
$ /proc/sched_debug
$ /proc/timer_list
$ /proc/diskdump
$ /proc/slabinfo
$ /proc/kallsyms
$ /proc/partitions
$ /proc/kcore
$ /proc/uptime
$ /proc/modules

$ /proc/cmdline
> kernel command line
```
    $ /proc/iomem
I/O memory map in Linux 2.4.

[proc_iomem(5)](https://man7.org/linux/man-pages/man5/proc_iomem.5.html)

``` console
$ /proc/<pid>/cmdline
$ /proc/<pid>/status
$ /proc/<pid>/maps
$ /proc/<pid>/coredump_filter
$ /proc/<pid>/oom_score
$ /proc/<pid>/oom_adj
$ /proc/<pid>/stat
$ /proc/<pid>/stack
```

    $ /proc/<pid>/task
To find the number of threads for a process, there's one more method that uses the same /proc directory. Just as every process has a directory created under its PID, every thread has a directory created under its thread ID. This is found in the /proc/\<pid>/task directory.

[proc_pid_limits(5)](https://man7.org/linux/man-pages/man5/proc_pid_limits.5.html)
-
    $ /poc/<pid>/limits
This file displays the soft limit, hard limit, and units of measurement for each of the process's resource limits



[proc_sysrq-trigger(5)](https://man7.org/linux/man-pages/man5/proc_sysrq-trigger.5.html)
-
``` console
# /proc/sys/kernel/sysrq
$ echo b > /proc/sysrq-trigger
```
Common Command Keys:

The character written to /proc/sysrq-trigger acts as the <command key> in the Alt + SysRq + <command key> combination.

Some common command keys include:

* b: Immediately reboots the system without syncing or unmounting disks.
* c: Triggers a system crash by dereferencing a NULL pointer (useful for crash dump testing).
* s: Synchronizes all mounted filesystems.
* u: Unmounts all mounted filesystems.
* e: Sends a SIGTERM signal to all processes except init.
* i: Sends a SIGKILL signal to all processes except init.
* k: Kills all processes on the current virtual console.
* o: Powers off the system.
* t: Dumps a list of current tasks and their states.
* m: Dumps memory information.
* p: Dumps current CPU registers and flags.
* h: Displays SysRq help (list of available commands).




### Netwrok
``` console
$ /proc/net/arp
$ /proc/net/vlan/config
$ /proc/net/vlan/<interface>
$ /proc/net/bonding/config
$ /proc/net/igmp
$ /proc/net/snmp
$ /proc/net/ip_tables_names
$ /proc/net/nf_conntrack
$ /proc/net/tcp
$ /proc/net/unix
$ /proc/sys/net/core/rmem_max(SO_RCVBUF)
$ /proc/sys/net/core/wmem_max(SO_SNDBUF)
$ /proc/sys/net/ipv4/ip_forward
$ /proc/sys/net/ipv4/tcp_tw_recycle
$ /proc/sys/net/ipv4/route/flush
$ /proc/sys/net/ipv4/tcp_tw_reuse
$ /proc/sys/net/ipv4/neigh/<dev>/gc_stale_time
$ /proc/sys/net/unix/max_dgram_qlen
$ /proc/sys/net/netfilter/nf_conntrack_max
$ /proc/sys/net/netfilter/nf_conntrack_count
```

### Interrupt
```
$ /proc/interrupts
$ /proc/irq/<IRQ_NUMBER>/smp_affinity
```

### Kernel
```
$ /proc/sys/kernel/core_pattern
$ /proc/sys/kernel/printk
$ /proc/sys/vm/oom_dump_tasks
$ /proc/sys/vm/drop_caches
```

### [msgrcv](https://manpages.ubuntu.com/manpages/trusty/man2/msgrcv.2.html)
```
$ /proc/sys/kernel/msgmnb
$ /proc/sys/kernel/msgmax
$ /proc/sys/fs/mqueue/queues_max
$ /proc/sysvipc/msg
```
    
### memory manage
```
$ cat /proc/meminfo
```
    
#### overcommit & oom killer
```
/proc/sys/vm/overcommit_memory
/proc/<pid>/oom_adj
/proc/<pid>/oom_score
```

## [sysfs](https://man7.org/linux/man-pages/man5/sysfs.5.html)
``` console
/sys/devices/system/cpu/cpu<N>/cache/index2/shared_cpu_list
/sys/devices/system/cpu/cpu<N>/cpufreq/

/sys/power/stat
/sys/bus/usb/devices/<usb id>/power/
/sys/module/usbcore/parameters/
    
/sys/kernel/kexec_crash_loaded
/sys/kernel/kexec_crash_size
/sys/kernel/mm/ksm/
/sys/kernel/slab/kmem_cache/free_calls
/sys/kernel/slab/kmalloc-4096/total_objects

/sys/devices/virtual/net/<bridge>/bridge/multicast_querier
/sys/class/net/br3/brif/<if name>/multicast_router
/sys/class/net/br0/brif/eth0.3/port_no
/sys/devices/virtual/net/br1/bridge/multicast_snooping
``` 
    
## debugfs
```
mount -t debugfs debugfs /sys/kernel/debug

/sys/kernel/debug/kmemleak
echo scan > /sys/kernel/debug/kmemleak
echo clear > /sys/kernel/debug/kmemleak
cat /sys/kernel/debug/kmemleak
echo scan=off > /sys/kernel/debug/kmemleak
echo off > /sys/kernel/debug/kmemleak

/sys/kernel/debug/slab/<cache>/ 
```

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
```
/dev/watchdog
```
[/dev/mem](https://man7.org/linux/man-pages/man4/mem.4.html)
> /dev/mem is a character device file that is an image of the main
memory of the computer.  It may be used, for example, to examine
(and even patch) the system.
>
>Byte addresses in /dev/mem are interpreted as physical memory
addresses.  References to nonexistent locations cause errors to be
returned.

## else
```
cat /etc/shells
/dev/random
```# File System Hierarchy Standard
The [Filesystem Hierarchy Standard](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard) (FHS) is a reference describing the conventions used for the layout of Unix-like systems. It has been made popular by its use in Linux distributions, but it is used by other Unix-like systems as well. It is maintained by the Linux Foundation. The latest version is 3.0, released on 3 June 2015.


## [procfs](https://man7.org/linux/man-pages/man5/proc.5.html)
``` console
$ /proc/cpuinfo
$ /proc/kmsg
$ /proc/mounts
$ /proc/timer_stats
$ /proc/sched_debug
$ /proc/timer_list
$ /proc/diskdump
$ /proc/slabinfo
$ /proc/kallsyms
$ /proc/partitions
$ /proc/kcore
$ /proc/uptime
$ /proc/modules

$ /proc/cmdline
> kernel command line
```
    $ /proc/iomem
I/O memory map in Linux 2.4.

[proc_iomem(5)](https://man7.org/linux/man-pages/man5/proc_iomem.5.html)

``` console
$ /proc/<pid>/cmdline
$ /proc/<pid>/status
$ /proc/<pid>/maps
$ /proc/<pid>/coredump_filter
$ /proc/<pid>/oom_score
$ /proc/<pid>/oom_adj
$ /proc/<pid>/stat
$ /proc/<pid>/stack
```

    $ /proc/<pid>/task
To find the number of threads for a process, there's one more method that uses the same /proc directory. Just as every process has a directory created under its PID, every thread has a directory created under its thread ID. This is found in the /proc/\<pid>/task directory.

[proc_pid_limits(5)](https://man7.org/linux/man-pages/man5/proc_pid_limits.5.html)
-
    $ /poc/<pid>/limits
This file displays the soft limit, hard limit, and units of measurement for each of the process's resource limits



[proc_sysrq-trigger(5)](https://man7.org/linux/man-pages/man5/proc_sysrq-trigger.5.html)
-
``` console
# /proc/sys/kernel/sysrq
$ echo b > /proc/sysrq-trigger
```
Common Command Keys:

The character written to /proc/sysrq-trigger acts as the <command key> in the Alt + SysRq + <command key> combination.

Some common command keys include:

* b: Immediately reboots the system without syncing or unmounting disks.
* c: Triggers a system crash by dereferencing a NULL pointer (useful for crash dump testing).
* s: Synchronizes all mounted filesystems.
* u: Unmounts all mounted filesystems.
* e: Sends a SIGTERM signal to all processes except init.
* i: Sends a SIGKILL signal to all processes except init.
* k: Kills all processes on the current virtual console.
* o: Powers off the system.
* t: Dumps a list of current tasks and their states.
* m: Dumps memory information.
* p: Dumps current CPU registers and flags.
* h: Displays SysRq help (list of available commands).




### Netwrok
``` console
$ /proc/net/arp
$ /proc/net/vlan/config
$ /proc/net/vlan/<interface>
$ /proc/net/bonding/config
$ /proc/net/igmp
$ /proc/net/snmp
$ /proc/net/ip_tables_names
$ /proc/net/nf_conntrack
$ /proc/net/tcp
$ /proc/net/unix
$ /proc/sys/net/core/rmem_max(SO_RCVBUF)
$ /proc/sys/net/core/wmem_max(SO_SNDBUF)
$ /proc/sys/net/ipv4/ip_forward
$ /proc/sys/net/ipv4/tcp_tw_recycle
$ /proc/sys/net/ipv4/route/flush
$ /proc/sys/net/ipv4/tcp_tw_reuse
$ /proc/sys/net/ipv4/neigh/<dev>/gc_stale_time
$ /proc/sys/net/unix/max_dgram_qlen
$ /proc/sys/net/netfilter/nf_conntrack_max
$ /proc/sys/net/netfilter/nf_conntrack_count
```

### Interrupt
```
$ /proc/interrupts
$ /proc/irq/<IRQ_NUMBER>/smp_affinity
```

### Kernel
```
$ /proc/sys/kernel/core_pattern
$ /proc/sys/kernel/printk
$ /proc/sys/vm/oom_dump_tasks
$ /proc/sys/vm/drop_caches
```

### [msgrcv](https://manpages.ubuntu.com/manpages/trusty/man2/msgrcv.2.html)
```
$ /proc/sys/kernel/msgmnb
$ /proc/sys/kernel/msgmax
$ /proc/sys/fs/mqueue/queues_max
$ /proc/sysvipc/msg
```
    
### memory manage
```
$ cat /proc/meminfo
```
    
#### overcommit & oom killer
```
/proc/sys/vm/overcommit_memory
/proc/<pid>/oom_adj
/proc/<pid>/oom_score
```

## [sysfs](https://man7.org/linux/man-pages/man5/sysfs.5.html)
``` console
/sys/devices/system/cpu/cpu<N>/cache/index2/shared_cpu_list
/sys/devices/system/cpu/cpu<N>/cpufreq/

/sys/power/stat
/sys/bus/usb/devices/<usb id>/power/
/sys/module/usbcore/parameters/
    
/sys/kernel/kexec_crash_loaded
/sys/kernel/kexec_crash_size
/sys/kernel/mm/ksm/
/sys/kernel/slab/kmem_cache/free_calls
/sys/kernel/slab/kmalloc-4096/total_objects

/sys/devices/virtual/net/<bridge>/bridge/multicast_querier
/sys/class/net/br3/brif/<if name>/multicast_router
/sys/class/net/br0/brif/eth0.3/port_no
/sys/devices/virtual/net/br1/bridge/multicast_snooping
``` 
    
## debugfs
```
mount -t debugfs debugfs /sys/kernel/debug

/sys/kernel/debug/kmemleak
echo scan > /sys/kernel/debug/kmemleak
echo clear > /sys/kernel/debug/kmemleak
cat /sys/kernel/debug/kmemleak
echo scan=off > /sys/kernel/debug/kmemleak
echo off > /sys/kernel/debug/kmemleak

/sys/kernel/debug/slab/<cache>/ 
```

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
```
/dev/watchdog
```
[/dev/mem](https://man7.org/linux/man-pages/man4/mem.4.html)
> /dev/mem is a character device file that is an image of the main
memory of the computer.  It may be used, for example, to examine
(and even patch) the system.
>
>Byte addresses in /dev/mem are interpreted as physical memory
addresses.  References to nonexistent locations cause errors to be
returned.

## else
```
cat /etc/shells
/dev/random
```