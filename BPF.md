# \(c\)BPF/eBPF
###### tags: `Linux`


#include <linux/bpf.h>

int bpf(int cmd, union bpf_attr *attr, unsigned int size);


linux/samples/bpf/cpustat_user.c




echo 'p:irene _do_fork' > /sys/kernel/debug/tracing/kprobe_events

cat /sys/kernel/debug/tracing/events/kprobes/irene/id

cat /sys/kernel/debug/tracing/trace_pipe

cat /sys/kernel/debug/tracing/trace

git clone https://github.com/llvm/llvm-project.git

make M=samples/bpf LLC=~/Github/llvm-project/llvm/build/bin/llc CLANG=~/Github/llvm-project/llvm/build/bin/clang

 ~/Github/llvm-project/llvm/build/bin/llvm-objdump --arch-name=bpf -S test_bpf.o

remove all events:
echo>/sys/kernel/debug/tracing/kprobe_events


## bpftrace
bpftrace [OPTIONS] -e 'program code'
```
't::' = tracepoint
'k::' = kprobe
'kr::' = kprobe return
```


## Reference
http://terenceli.github.io/%E6%8A%80%E6%9C%AF/2020/01/18/ebpf-in-c