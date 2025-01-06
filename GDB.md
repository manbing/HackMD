# GDB (GNU DeBug)
###### tags: `GNU`

si (execute single instruction)
stepi
next
step
list <line|function>
disas <function>



## GDBserver
gdbserver <ip>:<port> --attach <pid>
gdbserver <ip>:<port> <program>

## GDB
generate-core-file [file]
set sysroot <system root path>
info proc mapings
dump memory <file name> <start address> <end address>
backtrace
print <variable>
info sources
info functions
target remote <ip>:<port>
target remote :1234

### break
info breakpoints
delete breakpoints
disable <index>
enable <index>
break *\<address\>

break x:20 if strcmp(y, "hello") == 0
20 is line number, x can be any filename and y can be any variable.

break iter.c:6 if i == 5

### info
info line [File]:[Line]
info line [Function]
info line [File]:[Function]
info line *[Address]

### Print
print &((struct irene *)0x7ab2d8)->irene

### Examining Memory
x/nfu <addr>

### Register
info registers <span class="green">*<register name>*</span>
set $sstatus = 0xFFFFFFFF

### Variabler
set var value = 2148294706

### Reference
<style>
.green {
  color: #7FFF00;
}
</style>