---
title: Linux Debugging
tags: [Linux]

---

# Tools
1. addressToLine
``` console
$ addr2line -e ./oops_tryv2.o -p -f 0x124
do_the_work at <...>/ch7/oops_tryv2/oops_tryv2.c:62
```

2. scripts/checkstack.pl
``` console
$ objdump -d <...>/linux-5.10.60/vmlinux | <...>/linux-5.10.60/
scripts/checkstack.pl
0xffffffff810002100 sev_verify_cbit [vmlinux]:        4096
0xffffffff81a554300 od_set_powersave_bias [vmlinux]:  2064
0xffffffff817b24100 update_balloon_stats [vmlinux]:   1776
[...]
```

## Compile Option
[GCC Debugging-Options](https://gcc.gnu.org/onlinedocs/gcc/Debugging-Options.html)
``` console
-DDEBUG -g -ggdb -gdwarf-4 -Og -Wall
-fno-omit-frame-pointer -fvar-tracking-assignments
```

# Memory
[Debug linux kernel slab memory leak problem](https://www.cnblogs.com/WangYangkai/p/14442558.html)
[一次解决Linux内核内存泄漏实战全过程](https://zhuanlan.zhihu.com/p/577973486)
[Linux 核心設計: Kernel Debugging(1): Kdump](https://hackmd.io/@RinHizakura/HkHacces6)


``` console
$ cat /proc/slabinfo
$ grep "^Vm.*:" /proc/*/status
$ grep -r "^VmRSS" /proc/*/status |sed 's/kB$//'|sort -t: -k3n |tail

$ apt install bpfcc-tools
$ slabratetop-bpfcc -5
```
## [crash](https://man7.org/linux/man-pages/man8/crash.8.html)
Analyze Linux crash dump data or a live system

**Install vmlinux with debug symbols**
* Install [Debug symbol packages](https://documentation.ubuntu.com/server/explanation/debugging/debug-symbol-packages/)
``` console
sudo apt install ubuntu-dbgsym-keyring
```

``` console
echo "Types: deb
URIs: http://ddebs.ubuntu.com/
Suites: $(lsb_release -cs) $(lsb_release -cs)-updates $(lsb_release -cs)-proposed 
Components: main restricted universe multiverse
Signed-by: /usr/share/keyrings/ubuntu-dbgsym-keyring.gpg" | \
sudo tee -a /etc/apt/sources.list.d/ddebs.sources
```


``` cosole
sudo apt update
sudo apt install linux-image-$(uname -r)-dbgsym
```

``` cosole
crash /usr/lib/debug/boot/vmlinux-6.11.0-28-generic /dev/mem
```

---------------------------------------------------------------
# Save Linux Kernel Panic/Oops Log
pstore, mtdoops, crashlog, kdump

## [kdump](https://www.kernel.org/doc/html/latest/admin-guide/kdump/kdump.html)

## mtdoops


linux/drivers/mtd/mtdoops.c
``` c
static char mtddev[80] = "mtdoops";
module_param_string(mtddev, mtddev, 80, 0400);
MODULE_PARM_DESC(mtddev,
        "name or index number of the MTD device to use");
```


``` vi
CONFIG_MTD_OOPS=y
CONFIG_MAGIC_SYSRQ=y
```


![image](https://hackmd.io/_uploads/ryLwhTpwxx.png)


``` console
$ dd if=/dev/mtdblock1 of=/tmp/mtd1.bin
$ nanddump -f /tmp/mtd0.bin /dev/mtd17
$ mtd erase mtdoops
$ insmod mtdoops.ko mtddev=mtdoops
```


## pstore
``` vi
CONFIG_PSTORE=y
CONFIG_PSTORE_CONSOLE=y
CONFIG_PSTORE_PMSG=y
CONFIG_MAGIC_SYSRQ=y
```

![image](https://hackmd.io/_uploads/HJQ9napvll.png)



## crashlog


# [ftrace](https://hackmd.io/@manbing/ryfa9EKkw)
``` console
-p
--profile
-fprofile
-pg
```
Generate extra code to write profile information suitable for the analysis program prof (for -p, --profile, and -fprofile) or gprof (for -pg). You must use this option when compiling the source files you want data about, and you must also use it when linking.

You can use the function attribute no_instrument_function to suppress profiling of individual functions when compiling with these options.

# [\(c\)BPF/eBPF](https://hackmd.io/MoIbUgwzRbe-TSQ6NllufA)


# [oprofile](https://man7.org/linux/man-pages/man1/oprofile.1.html)

[HOWTO_Use_oprofile](https://linuxlink.timesys.com/docs/wiki/engineering/HOWTO_Use_oprofile)


# [valgrind](https://valgrind.org/docs/manual/manual.html)

# Kprobes
``` console
$ sudo kprobe-perf -s 'p:kmem_cache_alloc name=+0(+96(%di)):string' | grep -A10 "name=.*vm_area_struct"
```

# printk

``` console
$ echo 7 4 1 7 > /proc/sys/kernel/printk
$ echo 10 > /proc/sys/kernel/printk_ratelimit_burst
$ echo 5 > /proc/sys/kernel/printk_ratelimit
$ echo 500 > /proc/sys/kernel/printk_delay
```

``` c
// include/linux/printk.h
define printk_once(fmt, ...)                   \
    DO_ONCE_LITE(printk, fmt, ##__VA_ARGS__)
```

# Locking
KCSAN

KCSAN makes the `/sys/kernel/debug/kcsan` pseudofile available (under debugfs).

``` console
 /sys/kernel/debug/kcsan
```

# [Tracing](https://hackmd.io/xKfbS2JkQmeZ7t_ywyloSw?view)# Tools
1. addressToLine
``` console
$ addr2line -e ./oops_tryv2.o -p -f 0x124
do_the_work at <...>/ch7/oops_tryv2/oops_tryv2.c:62
```

2. scripts/checkstack.pl
``` console
$ objdump -d <...>/linux-5.10.60/vmlinux | <...>/linux-5.10.60/
scripts/checkstack.pl
0xffffffff810002100 sev_verify_cbit [vmlinux]:        4096
0xffffffff81a554300 od_set_powersave_bias [vmlinux]:  2064
0xffffffff817b24100 update_balloon_stats [vmlinux]:   1776
[...]
```

## Compile Option
[GCC Debugging-Options](https://gcc.gnu.org/onlinedocs/gcc/Debugging-Options.html)
``` console
-DDEBUG -g -ggdb -gdwarf-4 -Og -Wall
-fno-omit-frame-pointer -fvar-tracking-assignments
```

# Memory
[Debug linux kernel slab memory leak problem](https://www.cnblogs.com/WangYangkai/p/14442558.html)
[一次解决Linux内核内存泄漏实战全过程](https://zhuanlan.zhihu.com/p/577973486)
[Linux 核心設計: Kernel Debugging(1): Kdump](https://hackmd.io/@RinHizakura/HkHacces6)


``` console
$ cat /proc/slabinfo
$ grep "^Vm.*:" /proc/*/status
$ grep -r "^VmRSS" /proc/*/status |sed 's/kB$//'|sort -t: -k3n |tail

$ apt install bpfcc-tools
$ slabratetop-bpfcc -5
```
## [crash](https://man7.org/linux/man-pages/man8/crash.8.html)
Analyze Linux crash dump data or a live system

**Install vmlinux with debug symbols**
* Install [Debug symbol packages](https://documentation.ubuntu.com/server/explanation/debugging/debug-symbol-packages/)
``` console
sudo apt install ubuntu-dbgsym-keyring
```

``` console
echo "Types: deb
URIs: http://ddebs.ubuntu.com/
Suites: $(lsb_release -cs) $(lsb_release -cs)-updates $(lsb_release -cs)-proposed 
Components: main restricted universe multiverse
Signed-by: /usr/share/keyrings/ubuntu-dbgsym-keyring.gpg" | \
sudo tee -a /etc/apt/sources.list.d/ddebs.sources
```


``` cosole
sudo apt update
sudo apt install linux-image-$(uname -r)-dbgsym
```

``` cosole
crash /usr/lib/debug/boot/vmlinux-6.11.0-28-generic /dev/mem
```

---------------------------------------------------------------
# Save Linux Kernel Panic/Oops Log
pstore, mtdoops, crashlog, kdump

## [kdump](https://www.kernel.org/doc/html/latest/admin-guide/kdump/kdump.html)

## mtdoops


linux/drivers/mtd/mtdoops.c
``` c
static char mtddev[80] = "mtdoops";
module_param_string(mtddev, mtddev, 80, 0400);
MODULE_PARM_DESC(mtddev,
        "name or index number of the MTD device to use");
```


``` vi
CONFIG_MTD_OOPS=y
CONFIG_MAGIC_SYSRQ=y
```


![image](https://hackmd.io/_uploads/ryLwhTpwxx.png)


``` console
$ dd if=/dev/mtdblock1 of=/tmp/mtd1.bin
$ nanddump -f /tmp/mtd0.bin /dev/mtd17
$ mtd erase mtdoops
$ insmod mtdoops.ko mtddev=mtdoops
```


## pstore
``` vi
CONFIG_PSTORE=y
CONFIG_PSTORE_CONSOLE=y
CONFIG_PSTORE_PMSG=y
CONFIG_MAGIC_SYSRQ=y
```

![image](https://hackmd.io/_uploads/HJQ9napvll.png)



## crashlog


# [ftrace](https://hackmd.io/@manbing/ryfa9EKkw)
``` console
-p
--profile
-fprofile
-pg
```
Generate extra code to write profile information suitable for the analysis program prof (for -p, --profile, and -fprofile) or gprof (for -pg). You must use this option when compiling the source files you want data about, and you must also use it when linking.

You can use the function attribute no_instrument_function to suppress profiling of individual functions when compiling with these options.

# [\(c\)BPF/eBPF](https://hackmd.io/MoIbUgwzRbe-TSQ6NllufA)


# [oprofile](https://man7.org/linux/man-pages/man1/oprofile.1.html)

[HOWTO_Use_oprofile](https://linuxlink.timesys.com/docs/wiki/engineering/HOWTO_Use_oprofile)


# [valgrind](https://valgrind.org/docs/manual/manual.html)

# Kprobes
``` console
$ sudo kprobe-perf -s 'p:kmem_cache_alloc name=+0(+96(%di)):string' | grep -A10 "name=.*vm_area_struct"
```

# printk

``` console
$ echo 7 4 1 7 > /proc/sys/kernel/printk
$ echo 10 > /proc/sys/kernel/printk_ratelimit_burst
$ echo 5 > /proc/sys/kernel/printk_ratelimit
$ echo 500 > /proc/sys/kernel/printk_delay
```

``` c
// include/linux/printk.h
define printk_once(fmt, ...)                   \
    DO_ONCE_LITE(printk, fmt, ##__VA_ARGS__)
```

# Locking
KCSAN

KCSAN makes the `/sys/kernel/debug/kcsan` pseudofile available (under debugfs).

``` console
 /sys/kernel/debug/kcsan
```

# [Tracing](https://hackmd.io/xKfbS2JkQmeZ7t_ywyloSw?view)