# Qemu

qemu-system-riscv32 -machine help

qemu-system-riscv64 -M virt --machine dumpdtb=/tmp/dtb
dtc -I dtb -O dts /tmp/dtb -o /tmp/dts

## mode switch, Qemu/Monitor
Control+A, C

## gdb
In order to use gdb, launch QEMU with the -s and -S options. The -s option will make QEMU listen for an incoming connection from gdb on TCP port 1234, and -S will make QEMU not start the guest until you tell it to from gdb. (If you want to specify which TCP port to use or to use something other than TCP for the gdbstub connection, use the -gdb dev option instead of -s. See Using unix sockets for an example.)

>qemu-system-x86_64 -s -S -kernel bzImage -hda rootdisk.img -append "root=/dev/hda"