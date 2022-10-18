# Linux Tracing
###### tags: `Linux`

## Introduction

| Years | Technical |
| -------- | -------- |
| 2004     | kprobes/kretprobes     |
| 2005     | systemtap     |
| 2008     | ftrace     |
| 2009     | perf_events     |
| 2009     | tracepoints     |
| 2012     | uprobes     |
| 2015     | ebpf(Linux 4.1+)     |

## Kernel - Tracing Technical

### utrace

### kprobes/kretprobes

kprobes 主要用来对内核进行调试追踪, 属于比较轻量级的机制, 本质上是在指定的探测点(比如函数的某行, 函数的入口地址和出口地址, 或者内核的指定地址处)插入一组处理程序. 内核执行到这组处理程序的时候就可以获取到当前正在执行的上下文信息, 比如当前的函数名, 函数处理的参数以及函数的返回值, 也可以获取到寄存器甚至全局数据结构的信息.

kretprobes 在 kprobes 的机制上实现, 主要用于返回点(比如内核函数或者系统调用的返回值)的探测以及函数执行耗时的计算.

it can gets function symbol from many files.
in runtime, it can be gotten from file, /proc/kallsyms; in build system, it can be gotten from file, System.map.

#### kprobes kernel module


### systemtap

systemtap 其实已经存在很长时间了, 不过一直没有合并到内核主版本中, 这意味着它必须紧跟内核的变化, 每个版本的变动, 都需要做相应的调整, 这种方式也直接造成了我们难以在线上大规模使用 systemtap. 不过 systemtap 提供了很成熟的调试符号及复杂的探针处理程序, 支持对 tracepoint, kprobes 和 uprobes 的处理, 同时也可以进行内核编程, 以及性能相关的统计分析. 所以从大的方面来看, systemtap 可以在系统调用, 用户空间以及内核空间几个方面实现细粒度的跟踪分析, 另外 systemtap 也实现了自己的脚本语言, 方便 systemtap 将这些脚本工具转换为内核模块加载运行. 更详细的介绍可以参考春哥的文章 dynamic-tracing.


### ftrace

ftrace(function trace) 则更像是一个完整的追踪框架, 可以支持对 tracepoint, kprobes, uprobes 机制的处理, 同时还提供了事件追踪(event tracing, 类似 tracepoint 和 function trace 的组合) , 追踪过滤, 事件的计数和计时, 记录函数执行流程等功能. 我们常用的 perf-tools 工具集就是依赖 ftrace 机制而实现的.

虽然 ftrace 的内部是复杂的, 不过输出的信息却以简单明了为主. 其提供了一个基于文件系统(debugfs)的用户空间层面的 API 来方便大家执行各种跟踪和概要分析. 

#### kprobes on ftrace
* Linux kernel config
```
CONFIG_KPROBES=y
CONFIG_OPTPROBES=y
CONFIG_KPROBES_ON_FTRACE=y
CONFIG_UPROBES=y
CONFIG_KRETPROBES=y
CONFIG_HAVE_KPROBES=y
CONFIG_HAVE_KRETPROBES=y
CONFIG_HAVE_OPTPROBES=y
CONFIG_HAVE_KPROBES_ON_FTRACE=y
CONFIG_KPROBE_EVENT=y
```

* Debugfs
```
/sys/kernel/debug/tracing/kprobe_events-----------------------配置kprobe事件属性，增加事件之后会在kprobes下面生成对应目录。
/sys/kernel/debug/tracing/kprobe_profile----------------------kprobe事件统计属性文件。
/sys/kernel/debug/tracing/kprobes/<GRP>/<EVENT>/enabled-------使能kprobe事件
/sys/kernel/debug/tracing/kprobes/<GRP>/<EVENT>/filter--------过滤kprobe事件
/sys/kernel/debug/tracing/kprobes/<GRP>/<EVENT>/format--------查询kprobe事件显示格式
```

* Add kprobe event
```
p[:[GRP/]EVENT] [MOD:]SYM[+offs]|MEMADDR [FETCHARGS]-------------------设置一个probe探测点
r[:[GRP/]EVENT] [MOD:]SYM[+0] [FETCHARGS]------------------------------设置一个return probe探测点
-:[GRP/]EVENT----------------------------------------------------------删除一个探测点
```

* kprobe event setting 
```
GRP        : Group name. If omitted, use "kprobes" for it.------------设置后会在events/kprobes下创建<GRP>目录。
 EVENT        : Event name. If omitted, the event name is generated based on SYM+offs or MEMADDR.---指定后在events/kprobes/<GRP>生成<EVENT>目录。
 MOD        : Module name which has given SYM.--------------------------模块名，一般不设
 SYM[+offs]    : Symbol+offset where the probe is inserted.-------------被探测函数名和偏移
 MEMADDR    : Address where the probe is inserted.----------------------指定被探测的内存绝对地址
 FETCHARGS    : Arguments. Each probe can have up to 128 args.----------指定要获取的参数信息。
 %REG        : Fetch register REG---------------------------------------获取指定寄存器值
 @ADDR        : Fetch memory at ADDR (ADDR should be in kernel)--------获取指定内存地址的值
 @SYM[+|-offs]    : Fetch memory at SYM +|- offs (SYM should be a data symbol)---获取全局变量的值
 $stackN    : Fetch Nth entry of stack (N >= 0)----------------------------------获取指定栈空间值，即sp寄存器+N后的位置值
 $stack    : Fetch stack address.-----------------------------------------------获取sp寄存器值
 $retval    : Fetch return value.(*)--------------------------------------------获取返回值，用户return kprobe
 $comm        : Fetch current task comm.----------------------------------------获取对应进程名称。
 +|-offs(FETCHARG) : Fetch memory at FETCHARG +|- offs address.(**)-------------
 NAME=FETCHARG : Set NAME as the argument name of FETCHARG.
 FETCHARG:TYPE : Set TYPE as the type of FETCHARG. Currently, basic types (u8/u16/u32/u64/s8/s16/s32/s64), hexadecimal types
          (x8/x16/x32/x64), "string" and bitfield are supported.----------------设置参数的类型，可以支持字符串和比特类型
  (*) only for return probe.
  (**) this is useful for fetching a field of data structures.
```
执行如下两条命令就会生成目录/sys/kernel/debug/tracing/events/kprobes/myprobe；第三条命令则可以删除指定kprobe事件，如果要全部删除则echo > /sys/kernel/debug/tracing/kprobe_events。


参数后面的寄存器是跟架构相关的，%ax、%dx、%cx表示第1/2/3个参数，超出部分使用$stack来存储参数。

函数返回值保存在$retval中。

```
echo 'p:myprobe do_sys_open dfd=%ax filename=%dx flags=%cx mode=+4($stack)' > /sys/kernel/debug/tracing/kprobe_events
echo 'r:myretprobe do_sys_open ret=$retval' >> /sys/kernel/debug/tracing/kprobe_events-----------------------------------------------------这里面一定要用">>"，不然就会覆盖前面的设置。

echo '-:myprobe' >> /sys/kernel/debug/tracing/kprobe_events
echo '-:myretprobe' >> /sys/kernel/debug/tracing/kprobe_events
```



对kprobe事件的是能通过往对应事件的enable写1开启探测；写0暂停探测。

```
echo > /sys/kernel/debug/tracing/trace
echo 'p:myprobe do_sys_open dfd=%ax filename=%dx flags=%cx mode=+4($stack)' > /sys/kernel/debug/tracing/kprobe_events
echo 'r:myretprobe do_sys_open ret=$retval' >> /sys/kernel/debug/tracing/kprobe_events

echo 1 > /sys/kernel/debug/tracing/events/kprobes/myprobe/enable
echo 1 > /sys/kernel/debug/tracing/events/kprobes/myretprobe/enable
ls
echo 0 > /sys/kernel/debug/tracing/events/kprobes/myprobe/enable
echo 0 > /sys/kernel/debug/tracing/events/kprobes/myretprobe/enable

cat /sys/kernel/debug/tracing/trace
```

然后在/sys/kernel/debug/tracing/trace中可以看到结果。

```
sourceinsight4.-3356  [000] .... 3542865.754536: myprobe: (do_sys_open+0x0/0x290) dfd=0xffffffffbd6764a0 filename=0x8000 flags=0x1b6 mode=0xe3afff48ffffffff
            bash-26041 [001] .... 3542865.757014: myprobe: (do_sys_open+0x0/0x290) dfd=0xffffffffbd676460 filename=0x8241 flags=0x1b6 mode=0xe0c0ff48ffffffff
              ls-18078 [005] .... 3542865.757950: myprobe: (do_sys_open+0x0/0x290) dfd=0xffffffffbd676460 filename=0x88000 flags=0x1 mode=0xc1b7bf48ffffffff
              ls-18078 [005] d... 3542865.757953: myretprobe: (SyS_open+0x1e/0x20 <- do_sys_open) ret=0x3
              ls-18078 [005] .... 3542865.757966: myprobe: (do_sys_open+0x0/0x290) dfd=0xffffffffbd676460 filename=0x88000 flags=0x6168 mode=0xc1b7bf48ffffffff
              ls-18078 [005] d... 3542865.757969: myretprobe: (SyS_open+0x1e/0x20 <- do_sys_open) ret=0x3
              ls-18078 [005] .... 3542865.758001: myprobe: (do_sys_open+0x0/0x290) dfd=0xffffffffbd676460 filename=0x88000 flags=0x6168 mode=0xc1b7bf48ffffffff
              ls-18078 [005] d... 3542865.758004: myretprobe: (SyS_open+0x1e/0x20 <- do_sys_open) ret=0x3
              ls-18078 [005] .... 3542865.758030: myprobe: (do_sys_open+0x0/0x290) dfd=0xffffffffbd676460 filename=0x88000 flags=0x1000 mode=0xc1b7bf48ffffffff
              ls-18078 [005] d... 3542865.758033: myretprobe: (SyS_open+0x1e/0x20 <- do_sys_open) ret=0x3
              ls-18078 [005] .... 3542865.758055: myprobe: (do_sys_open+0x0/0x290) dfd=0xffffffffbd676460 filename=0x88000 flags=0x1000 mode=0xc1b7bf48ffffffff
              ls-18078 [005] d... 3542865.758057: myretprobe: (SyS_open+0x1e/0x20 <- do_sys_open) ret=0x3
              ls-18078 [005] .... 3542865.758080: myprobe: (do_sys_open+0x0/0x290) dfd=0xffffffffbd676460 filename=0x88000 flags=0x19d0 mode=0xc1b7bf48ffffffff
              ls-18078 [005] d... 3542865.758082: myretprobe: (SyS_open+0x1e/0x20 <- do_sys_open) ret=0x3
              ls-18078 [005] .... 3542865.758289: myprobe: (do_sys_open+0x0/0x290) dfd=0xffffffffbd676460 filename=0x8000 flags=0x1b6 mode=0xc1b7bf48ffffffff
              ls-18078 [005] d... 3542865.758297: myretprobe: (SyS_open+0x1e/0x20 <- do_sys_open) ret=0x3
              ls-18078 [005] .... 3542865.758339: myprobe: (do_sys_open+0x0/0x290) dfd=0xffffffffbd676460 filename=0x88000 flags=0x0 mode=0xc1b7bf48ffffffff
              ls-18078 [005] d... 3542865.758343: myretprobe: (SyS_open+0x1e/0x20 <- do_sys_open) ret=0x3
              ls-18078 [005] .... 3542865.758444: myprobe: (do_sys_open+0x0/0x290) dfd=0xffffffffbd676460 filename=0x98800 flags=0x2 mode=0xc1b7bf48ffffffff
              ls-18078 [005] d... 3542865.758446: myretprobe: (SyS_open+0x1e/0x20 <- do_sys_open) ret=0x3
            bash-26041 [001] .... 3542865.760416: myprobe: (do_sys_open+0x0/0x290) dfd=0xffffffffbd676460 filename=0x8241 flags=0x1b6 mode=0xe0c0ff48ffffffff
            bash-26041 [001] d... 3542865.760426: myretprobe: (SyS_open+0x1e/0x20 <- do_sys_open) ret=0x3
            bash-26041 [001] d... 3542865.793477: myretprobe: (SyS_open+0x1e/0x20 <- do_sys_open) ret=0x3
```

* 跟踪函数需要通过filter进行过滤，可以有效过滤掉冗余信息。

filter文件用于设置过滤条件，可以减少trace中输出的信息，它支持的格式和c语言的表达式类似，支持 ==，!=，>，<，>=，<=判断，并且支持与&&，或||，还有()。
```
echo 'filename==0x8241' > /sys/kernel/debug/tracing/events/kprobes/myprobe/filter
```
* 如果要在显示函数的同时显示其栈信息，可以通过配置trace_options来达到。

```
echo stacktrace > /sys/kernel/debug/tracing/trace_options
```

* 获取一段kprobe时间之后，可以再kprobe_profile中查看统计信息。

后面两列分别表示命中和未命中的次数。

```
cat /sys/kernel/debug/tracing/kprobe_profile 
  myprobe                                                   11               0
  myretprobe                                                11               0
```


### perf_events

perf_event 随内核的主版本进行发布, 一直是 linux 用户的主要追踪工具, 通常由 perf 命令提供服务. 可以支持对 tracepoint, kprobes 和 uprobes 机制的处理, 另外 perf 也是可以对 cpu 性能进行计数的强大工具之一. 值得一提的是 perf 可以将追踪的数据保存起来(默认为 perf.data) 方便以后分析, 这类似 tcpdump 的机制, 在分析存在延迟或者上下文切换的问题时尤为有用. Brendan Gregg 的 FlameGraph 性能火焰图 就是主要依靠 perf_event 的机制实现的.


### tracepoints

tracepoint 应该要比 ftrace 更早出现, 不过随着 ftrace 的完善, tracepoint 的机制也越来越成熟, 其本质上就是一种管理探测点(probe)和处理程序的机制, 管理员或者开发者可以动态的开启/关闭追踪功能. perf 和 ftrace 等工具也在很大程度上依赖了 tracepoint 特性.

### uprobes

### cbpf/ebpf

eBPF: extended Berkeley Packet Filter 已经被合并到了 Linux 内核的主版本中, 相当于一个内核虚拟机, 以 JIT(Just In Time) 的方式运行事件相关的追踪程序, 同时 eBPF 也支持对 ftrace, perf_events 等机制的处理. 另外 eBPF 在传统的包过滤器进行很大的变革, 其在内核追踪, 应用性能追踪, 流控等方面都做了很大的改变, 不过在接口的易用性方面还有待提高. 第三方的 bpftrace 实现了对 eBPF 的封装, 支持 python, lua 等接口, 用起来方便了很多, 还有其提供的 bcc 工具集在 > Linux 4.1+ 的系统中被广泛应用. 可以说 eBPF 能够监控所有想监控的, 在 Linux 4.1+ 系统中, 动态追踪工具使用 eBPF 一款即可. 低版本的内核更多的时候需要同时使用多个工具来互相辅助追踪分析.


## Userspace - Tracing Tools

### perf

### bpftrace / bcc

## Reference
[Linux 系统动态追踪技术介绍](https://blog.arstercz.com/introduction_to_linux_dynamic_tracing/)
[Linux kprobe调试技术使用](https://www.cnblogs.com/arnoldlu/p/9752061.html)