To run 32-bit ELF binaries on 64-bit platform, install `lib32z1` package.
``` console
$ sudo apt update
$ sudo apt install lib32z1
$ dpkg -L libc6-i386 | grep -w ld 
/lib32/ld-2.31.so
/lib/ld-linux.so.2
/lib32/ld-linux.so.2

$ /usr/lib/ld-linux.so.2 ./ncx32app
```

---
``` c
printk("%pF");
```


https://www.kernel.org/doc/html/latest/admin-guide/sysrq
