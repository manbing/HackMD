---
title: QEMU
tags: [Linux]

---

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

# Evironment
## OpenWrt, RISCV, SiFive FU540
[OpenWRT(5)：QEMU运行SiFive FU540(RISC-V)](https://www.cnblogs.com/arnoldlu/p/18338896)
[qemu-ifup script](https://www.linux-kvm.org/page/Networking)

1. Get source code:
``` shell
$ git clon https://github.com/openwrt/openwrt.git
```

2. Configure and Compile

* Set Target、Subtarget、Target Profile，以及生成ramdisk文件：
```
Target System (SiFive U-based RISC-V boards)
Subtarget (Generic)
Target Profile (SiFive Unleashed (FU540))
Target Images
　　ramdisk--編譯ramdisk版本。
```
* Add ethernet card
```
Kernel modules
　　Network Devices
　　　　kmod-e1000
```

* Add console
``` diff
diff --git a/target/linux/sifiveu/base-files/etc/inittab b/target/linux/sifiveu/base-files/etc/inittab
index 69f97c47c8..0d8ead1d91 100644
--- a/target/linux/sifiveu/base-files/etc/inittab
+++ b/target/linux/sifiveu/base-files/etc/inittab
@@ -1,4 +1,5 @@
 ::sysinit:/etc/init.d/rcS S boot
 ::shutdown:/etc/init.d/rcS K shutdown
 ttySIF0::askfirst:/usr/libexec/login.sh
+ttyS0::askfirst:/usr/libexec/login.sh
 tty1::askfirst:/usr/libexec/login.sh
```

3. Create `tap` inteface on host:
``` shell
$ sudo ip tuntap add dev tap0 mode tap
$ sudo ip address add dev tap0 192.168.2.1/24
$ sudo ip link set dev tap0 up
```

4. Execute QEMU:
``` shell
$ qemu-system-riscv64 -M virt \
-bios ./build_dir/target-riscv64_riscv64_musl/opensbi-generic/opensbi-2022-12-24-6b5188ca/build/platform/generic/firmware/fw_jump.elf \
-kernel ./build_dir/target-riscv64_riscv64_musl/linux-sifiveu_generic/Image-initramfs \
-append "rootwait root=/dev/ram0" \
-device e1000,netdev=n1 -netdev tap,id=n1,ifname=tap0 \
-nographic
```

5. Set inteface on OpenWrt:
``` shell
$ rctl delif br-lan eth0
$ ip addr add 192.168.2.2/24 dev eth0
$ ip link set eth0 up
```

## [OpenWrt, ARM, Raspberry pi 4b](https://hackmd.io/@sss22213/S1onYkIVK)

# Reference
[Running 64- and 32-bit RISC-V Linux on QEMU](https://risc-v-getting-started-guide.readthedocs.io/en/latest/linux-qemu.html)

[在QEMU上執行64 bit RISC-V Linux](https://medium.com/swark/%E5%9C%A8qemu%E4%B8%8A%E5%9F%B7%E8%A1%8C64-bit-risc-v-linux-2a527a078819)QEMU is a generic and open source machine emulator and virtualizer.


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

# Evironment
## OpenWrt, RISCV, SiFive FU540
[OpenWRT(5)：QEMU运行SiFive FU540(RISC-V)](https://www.cnblogs.com/arnoldlu/p/18338896)
[qemu-ifup script](https://www.linux-kvm.org/page/Networking)

1. Get source code:
``` shell
$ git clon https://github.com/openwrt/openwrt.git
```

2. Configure and Compile

* Set Target、Subtarget、Target Profile，以及生成ramdisk文件：
```
Target System (SiFive U-based RISC-V boards)
Subtarget (Generic)
Target Profile (SiFive Unleashed (FU540))
Target Images
　　ramdisk--編譯ramdisk版本。
```
* Add ethernet card
```
Kernel modules
　　Network Devices
　　　　kmod-e1000
```

* Add console
``` diff
diff --git a/target/linux/sifiveu/base-files/etc/inittab b/target/linux/sifiveu/base-files/etc/inittab
index 69f97c47c8..0d8ead1d91 100644
--- a/target/linux/sifiveu/base-files/etc/inittab
+++ b/target/linux/sifiveu/base-files/etc/inittab
@@ -1,4 +1,5 @@
 ::sysinit:/etc/init.d/rcS S boot
 ::shutdown:/etc/init.d/rcS K shutdown
 ttySIF0::askfirst:/usr/libexec/login.sh
+ttyS0::askfirst:/usr/libexec/login.sh
 tty1::askfirst:/usr/libexec/login.sh
```

3. Create `tap` inteface on host:
``` shell
$ sudo ip tuntap add dev tap0 mode tap
$ sudo ip address add dev tap0 192.168.2.1/24
$ sudo ip link set dev tap0 up
```

4. Execute QEMU:
``` shell
$ qemu-system-riscv64 -M virt \
-bios ./build_dir/target-riscv64_riscv64_musl/opensbi-generic/opensbi-2022-12-24-6b5188ca/build/platform/generic/firmware/fw_jump.elf \
-kernel ./build_dir/target-riscv64_riscv64_musl/linux-sifiveu_generic/Image-initramfs \
-append "rootwait root=/dev/ram0" \
-device e1000,netdev=n1 -netdev tap,id=n1,ifname=tap0 \
-nographic
```

5. Set inteface on OpenWrt:
``` shell
$ rctl delif br-lan eth0
$ ip addr add 192.168.2.2/24 dev eth0
$ ip link set eth0 up
```

## [OpenWrt, ARM, Raspberry pi 4b](https://hackmd.io/@sss22213/S1onYkIVK)

# Reference
[Running 64- and 32-bit RISC-V Linux on QEMU](https://risc-v-getting-started-guide.readthedocs.io/en/latest/linux-qemu.html)

[在QEMU上執行64 bit RISC-V Linux](https://medium.com/swark/%E5%9C%A8qemu%E4%B8%8A%E5%9F%B7%E8%A1%8C64-bit-risc-v-linux-2a527a078819)