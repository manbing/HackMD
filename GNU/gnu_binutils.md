---
title: GNU Binutils
tags: [GNU]

---

# GNU Binutils
###### tags: `GNU`

## addr2line
addr2line -e <span class="green">*filename*</span> [addr addr ...]
## ar
## objcopy
## objdump

The compile flag be placed in the section, .debug_str.
```
$ objdump -s --section .debug_str path/to/file.o
```

Show involved shared library
```
$ objdump -p /bin/ls
```

## readelf
Show involved shared library
```
readelf -d /bin/ls
```
## strings
## strip
## nm
## ld (The GNU linker)

**--verbose**
Display the version number for ld and list the linker emulations supported. Display which input files can and cannot be opened. <span class="green">Display the linker script being used by the linker</span>.

[Linker Scripts](https://ftp.gnu.org/old-gnu/Manuals/ld-2.9.1/html_chapter/ld_3.html#SEC6)

## ldd
Print shared object dependencies

## gcov

<style>
.green {
  color: #7FFF00;
}
</style>

# GNU Binutils
###### tags: `GNU`

## addr2line
addr2line -e <span class="green">*filename*</span> [addr addr ...]
## ar
## objcopy
## objdump

The compile flag be placed in the section, .debug_str.
```
$ objdump -s --section .debug_str path/to/file.o
```

Show involved shared library
```
$ objdump -p /bin/ls
```

## readelf
Show involved shared library
```
readelf -d /bin/ls
```
## strings
## strip
## nm
## ld (The GNU linker)

**--verbose**
Display the version number for ld and list the linker emulations supported. Display which input files can and cannot be opened. <span class="green">Display the linker script being used by the linker</span>.

[Linker Scripts](https://ftp.gnu.org/old-gnu/Manuals/ld-2.9.1/html_chapter/ld_3.html#SEC6)

## ldd
Print shared object dependencies

## gcov

<style>
.green {
  color: #7FFF00;
}
</style>

