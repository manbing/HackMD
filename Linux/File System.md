# File System
###### tags: `Linux`


## procfs [(5)](https://man7.org/linux/man-pages/man5/proc.5.html)

/proc/cpuinfo
/proc/<pid>/cmdline
/proc/<pid>/status
/proc/<pid>/maps
/proc/interrupts
/proc/sys/kernel/core_pattern
/proc/net/arp
/proc/kmsg
/proc/sys/net/core/rmem_max(SO_RCVBUF)
/proc/sys/net/core/wmem_max(SO_SNDBUF)
    
### memory manage
>cat /proc/meminfo
    
#### overcommit & oom killer
>/proc/sys/vm/overcommit_memory
/proc/<pid>/oom_adj
/proc/<pid>/oom_score

## sysfs

## debugfs
mount -t debugfs none /sys/kernel/debug
echo scan > /sys/kernel/debug/kmemleak
echo clear > /sys/kernel/debug/kmemleak
cat /sys/kernel/debug/kmemleak
echo scan=off > /sys/kernel/debug/kmemleak
echo off > /sys/kernel/debug/kmemleak

## bpf

## else
cat /etc/shells
/dev/random