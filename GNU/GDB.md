---
title: GDB (GNU DeBug)
tags: [GNU]

---

# GDB (GNU DeBug)
###### tags: `GNU`

 GDB initialization/startup file `~/.gdbinit`

``` shell
(gdb) next
(gdb) step
(gdb) list <line|function>
(gdb) disas <function>

(gdb) generate-core-file [file]
(gdb) set sysroot <system root path>
(gdb) info proc mapings

(gdb) print <variable>
(gdb) info sources
(gdb) info functions
(gdb) info sharedlibrary
(gdb) set solib-search-path <path>
(gdb) target remote <ip>:<port>
(gdb) target remote :1234

(gdb) add-symbol-file
(gdb) core-file <file name>
```

## shortcuts
* typing `Ctrl-C` will stop the program, and continue will resume it without sending any signal to it.

* typing `Ctrl-Z` will stop the program, and continue will resume it accompanied by a SIGTSTP signal, so it will immediately stop again. If you type continue again, it should resume.

    
## breakpointer
In most common debugging scenarios, you can simply use the `break` command, and GDB will automatically select the appropriate method. You only need to explicitly use `hbreak` if you encounter issues with standard breakpoints (e.g., when debugging embedded systems or kernels where memory regions are read-only). 

* Software Breakpoint (break):
* Hardware breakpoints (hbreak):
``` console
(gdb) info breakpoints
(gdb) delete breakpoints
(gdb) disable <index>
(gdb) enable <index>
(gdb) break *<address>
(gdb) break iter.c:6 if i == 5
```

* Temporary breakpoints:
The `tbreak` command sets up a breakpoint that will work only once. After that, it's automatically cleaned up. You don't have to remember to use the disable N or delete N GDB commands – useful when in a hurry.

``` console
(gdb) tbreak
```

* Conditional breakpoints:
`(gdb) break x:20 if strcmp(y, "hello") == 0`
20 is line number, `x` can be any filename and `y` can be any variable.

* Hardware watchpoints:
```
(gdb) watch jiffies_64
```

## info
``` shell
(gdb) info line [File]:[Line]
(gdb) info line [Function]
(gdb) info line [File]:[Function]
(gdb) info line *[Address]
```

## Print
``` shell
(gdb) print &((struct irene *)0x7ab2d8)->irene
```

## Examining Memory
``` shell
(gdb) x/nfu <addr>
(gdb) x/i $pc
(gdb) set *0xff800000=0x87
(gdb) x/b 0xff800000
(gdb) dump memory <file name> <start address> <end address>
(gdb) set print pretty
```

## Examining the Symbol Table
* `whatis *expr*`
Print the data type of expression expr. expr is not actually evaluated, and any side-effecting operations (such as assignments or function calls) inside it do not take place. See section Expressions.
* `whatis`
Print the data type of `$`, the last value in the value history.


## Register
``` shell
(gdb) info registers <register name>
(gdb) set $sstatus = 0xFFFFFFFF
(gdb) info registers
(gdb) info all-registers
```

## Variabler
``` shell
(gdb) set var value = 2148294706
```

## stack frame
the compile flags(CFLAGS), -fomit-frame-pointer, will cause stack frame can not trace able sometime, depend on microarchitecture.

``` shell
(gdb) info frame
(gdb) frame
(gdb) backtrace
(gdb) backtrace full
(gdb) frame <id>
```

## thread
``` shell
(gdb) info thread
(gdb) thread <id>
```

## Share library
``` console
(gdb) info sharedlibrary
(gdb) set solib-absolute-prefix <Path>
(gdb) set solib-search-path <Path>
(gdb) add-symbol-file <File> <Memmory address>
```

## add-symbol-file
Load library or kernel module

* Get virtual address
``` console
$ ls -a /sys/module/<module-name>/sections/
$ cd /sys/module/<module-name>/sections/
$ cat .text .rodata .data .bss
0xffffffffc033b000   0xffffffffc0348060 0xffffffffc034e000
0xffffffffc0354f00
```

* Load it into GDB
``` console
(gdb) add-symbol-file </path/to/>usbhid.ko 0xffffffffc033b000
\
      -s .rodata 0xffffffffc0348060 \
      -s .data 0xffffffffc034e000 \ [...]
```

* show all modules currently loaded
``` console
(gdb) lx-lsmod
Address            Module                  Size  Used by
0xffffffffc004a000 kgdb_try               20480  0
(gdb)
```

## GDBserver
--
``` shell
$ gdbserver <ip>:<port> --attach <pid>
$ gdbserver <ip>:<port> <program>
```

## Embedded System
``` console
(gdb) set sysroot <the root path of the filesystem>
(gdb) set solib-search-path <the libaray path of the filesystem>
```

## Text User Interface (TUI)
> GDB has a Text User Interface (TUI) mode, where, instead of just the usual command- line interface, the terminal window is split into two or three horizontally tiled panes
(it internally uses curses-based libraries and APIs to achieve its somewhat graphic-like capabilities).

``` console
$ gdb –tui –q linux-5.10.53/vmlinux
[...]
```
## Assembly-level Debug
single-step exactly one machine instruction! (You can also specify si N to tell GDB to step through N assembly-level instructions.)

``` console
(gdb) si (execute single instruction)
(gdb) stepi
(gdb) disas
```


## Reference
<style>
.green {
  color: #7FFF00;
}
</style># GDB (GNU DeBug)
###### tags: `GNU`

 GDB initialization/startup file `~/.gdbinit`

``` shell
(gdb) next
(gdb) step
(gdb) list <line|function>
(gdb) disas <function>

(gdb) generate-core-file [file]
(gdb) set sysroot <system root path>
(gdb) info proc mapings

(gdb) print <variable>
(gdb) info sources
(gdb) info functions
(gdb) info sharedlibrary
(gdb) set solib-search-path <path>
(gdb) target remote <ip>:<port>
(gdb) target remote :1234

(gdb) add-symbol-file
(gdb) core-file <file name>
```

## shortcuts
* typing `Ctrl-C` will stop the program, and continue will resume it without sending any signal to it.

* typing `Ctrl-Z` will stop the program, and continue will resume it accompanied by a SIGTSTP signal, so it will immediately stop again. If you type continue again, it should resume.

    
## breakpointer
In most common debugging scenarios, you can simply use the `break` command, and GDB will automatically select the appropriate method. You only need to explicitly use `hbreak` if you encounter issues with standard breakpoints (e.g., when debugging embedded systems or kernels where memory regions are read-only). 

* Software Breakpoint (break):
* Hardware breakpoints (hbreak):
``` console
(gdb) info breakpoints
(gdb) delete breakpoints
(gdb) disable <index>
(gdb) enable <index>
(gdb) break *<address>
(gdb) break iter.c:6 if i == 5
```

* Temporary breakpoints:
The `tbreak` command sets up a breakpoint that will work only once. After that, it's automatically cleaned up. You don't have to remember to use the disable N or delete N GDB commands – useful when in a hurry.

``` console
(gdb) tbreak
```

* Conditional breakpoints:
`(gdb) break x:20 if strcmp(y, "hello") == 0`
20 is line number, `x` can be any filename and `y` can be any variable.

* Hardware watchpoints:
```
(gdb) watch jiffies_64
```

## info
``` shell
(gdb) info line [File]:[Line]
(gdb) info line [Function]
(gdb) info line [File]:[Function]
(gdb) info line *[Address]
```

## Print
``` shell
(gdb) print &((struct irene *)0x7ab2d8)->irene
```

## Examining Memory
``` shell
(gdb) x/nfu <addr>
(gdb) x/i $pc
(gdb) set *0xff800000=0x87
(gdb) x/b 0xff800000
(gdb) dump memory <file name> <start address> <end address>
(gdb) set print pretty
```

## Examining the Symbol Table
* `whatis *expr*`
Print the data type of expression expr. expr is not actually evaluated, and any side-effecting operations (such as assignments or function calls) inside it do not take place. See section Expressions.
* `whatis`
Print the data type of `$`, the last value in the value history.


## Register
``` shell
(gdb) info registers <register name>
(gdb) set $sstatus = 0xFFFFFFFF
(gdb) info registers
(gdb) info all-registers
```

## Variabler
``` shell
(gdb) set var value = 2148294706
```

## stack frame
the compile flags(CFLAGS), -fomit-frame-pointer, will cause stack frame can not trace able sometime, depend on microarchitecture.

``` shell
(gdb) info frame
(gdb) frame
(gdb) backtrace
(gdb) backtrace full
(gdb) frame <id>
```

## thread
``` shell
(gdb) info thread
(gdb) thread <id>
```

## Share library
``` console
(gdb) info sharedlibrary
(gdb) set solib-absolute-prefix <Path>
(gdb) set solib-search-path <Path>
(gdb) add-symbol-file <File> <Memmory address>
```

## add-symbol-file
Load library or kernel module

* Get virtual address
``` console
$ ls -a /sys/module/<module-name>/sections/
$ cd /sys/module/<module-name>/sections/
$ cat .text .rodata .data .bss
0xffffffffc033b000   0xffffffffc0348060 0xffffffffc034e000
0xffffffffc0354f00
```

* Load it into GDB
``` console
(gdb) add-symbol-file </path/to/>usbhid.ko 0xffffffffc033b000
\
      -s .rodata 0xffffffffc0348060 \
      -s .data 0xffffffffc034e000 \ [...]
```

* show all modules currently loaded
``` console
(gdb) lx-lsmod
Address            Module                  Size  Used by
0xffffffffc004a000 kgdb_try               20480  0
(gdb)
```

## GDBserver
--
``` shell
$ gdbserver <ip>:<port> --attach <pid>
$ gdbserver <ip>:<port> <program>
```

## Embedded System
``` console
(gdb) set sysroot <the root path of the filesystem>
(gdb) set solib-search-path <the libaray path of the filesystem>
```

## Text User Interface (TUI)
> GDB has a Text User Interface (TUI) mode, where, instead of just the usual command- line interface, the terminal window is split into two or three horizontally tiled panes
(it internally uses curses-based libraries and APIs to achieve its somewhat graphic-like capabilities).

``` console
$ gdb –tui –q linux-5.10.53/vmlinux
[...]
```
## Assembly-level Debug
single-step exactly one machine instruction! (You can also specify si N to tell GDB to step through N assembly-level instructions.)

``` console
(gdb) si (execute single instruction)
(gdb) stepi
(gdb) disas
```


## Reference
<style>
.green {
  color: #7FFF00;
}
</style>