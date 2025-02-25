# Performance
###### tags: `Linux`

```
pidstat
vmstat
sar
ps
top
/proc/<pid>/stat
```


## Perf
    
### Perf-record
```
sudo perf record -g --call-graph fp -o ps.fp.perf.data ps
sudo perf report --stdio -i ps.fp.perf.data
sudo perf report --tui -i ps.fp.perf.data

sudo perf record -e probe:scsi_test_unit_ready -aR sleep 10
perf probe schedule:12 cpu
```
    

### Perf-stat
```
perf stat -a ./a.out
perf stat -e cache-misses <command>
perf stat -e dTLB-load-misses,iTLB-load-misses <command>
sudo perf stat -d ./array.out
sudo perf stat -ddd taskset --cpu-list 0 ./array.out
```

    
### Perf-list
```
sudo perf list tracepoint
```

### Perf-script
```
perf script -g python
perf script
sudo perf script -s perf-script.py
```
    
## function time
```
perf probe --line is_prime -x array
sudo perf probe -x array 'is_prime'
sudo perf probe -x array 'is_prime_end=is_prime:25'
perf record -e probe_array:is_prime -e probe_array:is_prime_end -aR sleep 10
```