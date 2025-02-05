---
title: GDB (GNU DeBug)
tags: [GNU]

---

# GDB (GNU DeBug)
###### tags: `GNU`


## GDBserver
```
gdbserver <ip>:<port> --attach <pid>
gdbserver <ip>:<port> <program>
```

## GDB
```
si (execute single instruction)
stepi
next
step
list <line|function>
disas <function>

generate-core-file [file]
set sysroot <system root path>
info proc mapings

print <variable>
info sources
info functions
info sharedlibrary
set solib-search-path <path>
target remote <ip>:<port>
target remote :1234

add-symbol-file
core-file <file name>
```

### shortcuts
* typing `Ctrl-C` will stop the program, and continue will resume it without sending any signal to it.

* typing `Ctrl-Z` will stop the program, and continue will resume it accompanied by a SIGTSTP signal, so it will immediately stop again. If you type continue again, it should resume.

    
### break
```
$ info breakpoints
$ delete breakpoints
$ disable <index>
$ enable <index>
$ break <address>
$ break iter.c:6 if i == 5
```

break x:20 if strcmp(y, "hello") == 0
> 20 is line number, `x` can be any filename and `y` can be any variable.

### info
```
$ info line [File]:[Line]
$ info line [Function]
$ info line [File]:[Function]
$ info line *[Address]
```

### Print
print &((struct irene *)0x7ab2d8)->irene

### Examining Memory
```
$ x/nfu $\lt$addr>
$ x/i $pc
$ set *0xff800000=0x87
$ x/b 0xff800000
$ dump memory <file name> <start address> <end address>
```

### Register
> $ info registers <span class="green">*<register name>*</span>
$ set $sstatus = 0xFFFFFFFF

### Variabler
set var value = 2148294706

### frame
```
$ info frame
$ frame
$ backtrace
$ backtrace full
$ frame <id>
```

### thread
```
$ info thread
$ thread <id>
```

### Reference
<style>
.green {
  color: #7FFF00;
}
</style># GDB (GNU DeBug)
###### tags: `GNU`


## GDBserver
```
gdbserver <ip>:<port> --attach <pid>
gdbserver <ip>:<port> <program>
```

## GDB
```
si (execute single instruction)
stepi
next
step
list <line|function>
disas <function>

generate-core-file [file]
set sysroot <system root path>
info proc mapings

print <variable>
info sources
info functions
info sharedlibrary
set solib-search-path <path>
target remote <ip>:<port>
target remote :1234

add-symbol-file
core-file <file name>
```

### shortcuts
* typing `Ctrl-C` will stop the program, and continue will resume it without sending any signal to it.

* typing `Ctrl-Z` will stop the program, and continue will resume it accompanied by a SIGTSTP signal, so it will immediately stop again. If you type continue again, it should resume.

    
### break
```
$ info breakpoints
$ delete breakpoints
$ disable <index>
$ enable <index>
$ break <address>
$ break iter.c:6 if i == 5
```

break x:20 if strcmp(y, "hello") == 0
> 20 is line number, `x` can be any filename and `y` can be any variable.

### info
```
$ info line [File]:[Line]
$ info line [Function]
$ info line [File]:[Function]
$ info line *[Address]
```

### Print
print &((struct irene *)0x7ab2d8)->irene

### Examining Memory
```
$ x/nfu $\lt$addr>
$ x/i $pc
$ set *0xff800000=0x87
$ x/b 0xff800000
$ dump memory <file name> <start address> <end address>
```

### Register
> $ info registers <span class="green">*<register name>*</span>
$ set $sstatus = 0xFFFFFFFF

### Variabler
set var value = 2148294706

### frame
```
$ info frame
$ frame
$ backtrace
$ backtrace full
$ frame <id>
```

### thread
```
$ info thread
$ thread <id>
```

### Reference
<style>
.green {
  color: #7FFF00;
}
</style>