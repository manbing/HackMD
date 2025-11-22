
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


``` vim
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
``` vim
CONFIG_PSTORE=y
CONFIG_PSTORE_CONSOLE=y
CONFIG_PSTORE_PMSG=y
CONFIG_MAGIC_SYSRQ=y
```

![image](https://hackmd.io/_uploads/HJQ9napvll.png)



## crashlog


# [ftrace](https://hackmd.io/@manbing/ryfa9EKkw)


# [\(c\)BPF/eBPF](https://hackmd.io/MoIbUgwzRbe-TSQ6NllufA)


# [oprofile](https://man7.org/linux/man-pages/man1/oprofile.1.html)

[HOWTO_Use_oprofile](https://linuxlink.timesys.com/docs/wiki/engineering/HOWTO_Use_oprofile)


# [valgrind](https://valgrind.org/docs/manual/manual.html)