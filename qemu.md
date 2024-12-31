QEMU is a generic and open source machine emulator and virtualizer.


```
$ qemu-system-riscv32 -machine help
$ qemu-system-riscv64 -M virt --machine dumpdtb=/tmp/dtb dtc -I dtb -O dts /tmp/dtb -o /tmp/dts
```
# mode switch, Qemu/Monitor
Control+A, C


# gdb
In order to use gdb, launch QEMU with the -s and -S options. The -s option will make QEMU listen for an incoming connection from gdb on TCP port 1234, and -S will make QEMU not start the guest until you tell it to from gdb. (If you want to specify which TCP port to use or to use something other than TCP for the gdbstub connection, use the -gdb dev option instead of -s. See Using unix sockets for an example.)
```
$ qemu-system-x86_64 -s -S -kernel bzImage -hda rootdisk.img -append "root=/dev/hda"
```

# OpenWrt, RISCV, SiFive FU540
create `tap` inteface on host:
```
$ sudo ip tuntap add dev tap0 mode tap
sudo ip address add dev tap0 192.168.2.1/24
sudo ip link set dev tap0 up
```

execute qemu:
```
$ qemu-system-riscv64 -M virt \
-bios ./build_dir/target-riscv64_riscv64_musl/opensbi-generic/opensbi-2022-12-24-6b5188ca/build/platform/generic/firmware/fw_jump.elf \
-kernel ./build_dir/target-riscv64_riscv64_musl/linux-sifiveu_generic/Image-initramfs \
-append "rootwait root=/dev/ram0" \
-device e1000,netdev=n1 -netdev tap,id=n1,ifname=tap0 \
-nographic
```

[OpenWRT(5)：QEMU运行SiFive FU540(RISC-V)](https://www.cnblogs.com/arnoldlu/p/18338896)
[qemu-ifup script](https://www.linux-kvm.org/page/Networking)