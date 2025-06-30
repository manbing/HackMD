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

Prerequest:
<mark>qemu-system-riscv64</mark>
<mark>OpneWrt</mark>

**Generate OpenWrt image**
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

**Execute QEMU**
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
Prerequest:
<mark>qemu-system-aarch64</mark>
<mark>OpneWrt</mark>

**qemu-system-aarch64**
1. Get source code and compile
``` console
$ git clone  https://gitlab.com/qemu-project/qemu.git
$ cd qemu
$ git switch -c v9.0.0 v9.0.0
$ mkdir build
$ cd build
$ ../configure --target-list=aarch64-softmmu
$ make
$ make install
```

``` console
QEMU emulator version 9.0.0 (v9.0.0)
Copyright (c) 2003-2024 Fabrice Bellard and the QEMU Project developers
```

**Generate OpenWrt image**
1. Get source code and compile
``` console
$ git clone https://github.com/openwrt/openwrt.git
$ ./scripts/feeds update -a
$ ./scripts/feeds install -a
$ make menuconfig
```
``` vim
CONFIG_TARGET_BOARD="bcm27xx"
CONFIG_TARGET_SUBTARGET="bcm2711"
CONFIG_TARGET_PROFILE="DEVICE_rpi-4"
CONFIG_TARGET_ARCH_PACKAGES="aarch64_cortex-a72"
CONFIG_DEFAULT_TARGET_OPTIMIZATION="-Os -pipe"
CONFIG_CPU_TYPE="cortex-a72"
```
``` console
$ make V=s -j$(nproc)
```
2. Decompress image
``` console
$ cd bin/targets/bcm27xx/bcm2711
$ gunzip -d openwrt-bcm27xx-bcm2711-rpi-4-ext4-factory.img.gz
```

**Execute QEMU**
``` console
$ qemu-system-aarch64 \
    -M raspi4b \
    -m 2048 \
    -kernel ./build_dir/target-aarch64_cortex-a72_musl/linux-bcm27xx_bcm2711/linux-5.15.134/arch/arm64/boot/Image \
    -dtb ./build_dir/target-aarch64_cortex-a72_musl/linux-bcm27xx_bcm2711/linux-5.15.134/arch/arm64/boot/dts/broadcom/bcm2711-rpi-4-b.dtb \
    -append "root=/dev/mmcblk0p2 rootfstype=ext4 console=ttyAMA0 earlycon= pl011,0xfe201000 ip=192.168.7.2::192.168.7.1:255.255.255.0:rpi4:eth0:off" \
    -drive file=./bin/targets/bcm27xx/bcm2711/openwrt-bcm27xx-bcm2711-rpi-4-ext4-factory.img,if=sd,format=raw \
    -nographic \
    -net nic,model=e1000 -net tap,ifname=tp0,script=no,downscript=no
```


## OpenWrt, ARM, Virt

**.config**
``` vim
CONFIG_TARGET_BOARD="bcm27xx"
CONFIG_TARGET_SUBTARGET="bcm2711"
CONFIG_TARGET_PROFILE="DEVICE_rpi-4"
CONFIG_TARGET_ARCH_PACKAGES="aarch64_cortex-a72"
CONFIG_DEFAULT_TARGET_OPTIMIZATION="-Os -pipe"
CONFIG_CPU_TYPE="cortex-a72"
CONFIG_TARGET_ROOTFS_INITRAMFS=y
CONFIG_TARGET_INITRAMFS_COMPRESSION_NONE=
```

**Execute QEMU**
``` console    
$ qemu-system-aarch64 -M virt -cpu cortex-a72 -m 512 \
    -nographic \
    -kernel ./bin/targets/bcm27xx/bcm2711/openwrt-bcm27xx-bcm2711-rpi-4-initramfs-kernel.bin
```

## OpenWrt, X86_64, Virt

**.config**
``` vim
CONFIG_TARGET_x86_64_DEVICE_generic=y
CONFIG_HAS_SUBTARGETS=y
CONFIG_HAS_DEVICES=y
CONFIG_TARGET_BOARD="x86"
CONFIG_TARGET_SUBTARGET="64"
CONFIG_TARGET_PROFILE="DEVICE_generic"
CONFIG_TARGET_ARCH_PACKAGES="x86_64"
CONFIG_DEFAULT_TARGET_OPTIMIZATION="-Os -pipe"
CONFIG_CPU_TYPE=" "
CONFIG_LINUX_5_15=y
```

**Execute QEMU**
``` console
$ qemu-system-x86_64 -smp 2 -m 1024 \
    -nographic \
    -append "console=ttyS0" \
    -kernel ./bin/targets/x86/64/openwrt-x86-64-generic-initramfs-kernel.bin
```


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

Prerequest:
<mark>qemu-system-riscv64</mark>
<mark>OpneWrt</mark>

**Generate OpenWrt image**
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

**Execute QEMU**
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
Prerequest:
<mark>qemu-system-aarch64</mark>
<mark>OpneWrt</mark>

**qemu-system-aarch64**
1. Get source code and compile
``` console
$ git clone  https://gitlab.com/qemu-project/qemu.git
$ cd qemu
$ git switch -c v9.0.0 v9.0.0
$ mkdir build
$ cd build
$ ../configure --target-list=aarch64-softmmu
$ make
$ make install
```

``` console
QEMU emulator version 9.0.0 (v9.0.0)
Copyright (c) 2003-2024 Fabrice Bellard and the QEMU Project developers
```

**Generate OpenWrt image**
1. Get source code and compile
``` console
$ git clone https://github.com/openwrt/openwrt.git
$ ./scripts/feeds update -a
$ ./scripts/feeds install -a
$ make menuconfig
```
``` vim
CONFIG_TARGET_BOARD="bcm27xx"
CONFIG_TARGET_SUBTARGET="bcm2711"
CONFIG_TARGET_PROFILE="DEVICE_rpi-4"
CONFIG_TARGET_ARCH_PACKAGES="aarch64_cortex-a72"
CONFIG_DEFAULT_TARGET_OPTIMIZATION="-Os -pipe"
CONFIG_CPU_TYPE="cortex-a72"
```
``` console
$ make V=s -j$(nproc)
```
2. Decompress image
``` console
$ cd bin/targets/bcm27xx/bcm2711
$ gunzip -d openwrt-bcm27xx-bcm2711-rpi-4-ext4-factory.img.gz
```

**Execute QEMU**
``` console
$ qemu-system-aarch64 \
    -M raspi4b \
    -m 2048 \
    -kernel ./build_dir/target-aarch64_cortex-a72_musl/linux-bcm27xx_bcm2711/linux-5.15.134/arch/arm64/boot/Image \
    -dtb ./build_dir/target-aarch64_cortex-a72_musl/linux-bcm27xx_bcm2711/linux-5.15.134/arch/arm64/boot/dts/broadcom/bcm2711-rpi-4-b.dtb \
    -append "root=/dev/mmcblk0p2 rootfstype=ext4 console=ttyAMA0 earlycon= pl011,0xfe201000 ip=192.168.7.2::192.168.7.1:255.255.255.0:rpi4:eth0:off" \
    -drive file=./bin/targets/bcm27xx/bcm2711/openwrt-bcm27xx-bcm2711-rpi-4-ext4-factory.img,if=sd,format=raw \
    -nographic \
    -net nic,model=e1000 -net tap,ifname=tp0,script=no,downscript=no
```


## OpenWrt, ARM, Virt

**.config**
``` vim
CONFIG_TARGET_BOARD="bcm27xx"
CONFIG_TARGET_SUBTARGET="bcm2711"
CONFIG_TARGET_PROFILE="DEVICE_rpi-4"
CONFIG_TARGET_ARCH_PACKAGES="aarch64_cortex-a72"
CONFIG_DEFAULT_TARGET_OPTIMIZATION="-Os -pipe"
CONFIG_CPU_TYPE="cortex-a72"
CONFIG_TARGET_ROOTFS_INITRAMFS=y
CONFIG_TARGET_INITRAMFS_COMPRESSION_NONE=
```

**Execute QEMU**
``` console    
$ qemu-system-aarch64 -M virt -cpu cortex-a72 -m 512 \
    -nographic \
    -kernel ./bin/targets/bcm27xx/bcm2711/openwrt-bcm27xx-bcm2711-rpi-4-initramfs-kernel.bin
```

## OpenWrt, X86_64, Virt

**.config**
``` vim
CONFIG_TARGET_x86_64_DEVICE_generic=y
CONFIG_HAS_SUBTARGETS=y
CONFIG_HAS_DEVICES=y
CONFIG_TARGET_BOARD="x86"
CONFIG_TARGET_SUBTARGET="64"
CONFIG_TARGET_PROFILE="DEVICE_generic"
CONFIG_TARGET_ARCH_PACKAGES="x86_64"
CONFIG_DEFAULT_TARGET_OPTIMIZATION="-Os -pipe"
CONFIG_CPU_TYPE=" "
CONFIG_LINUX_5_15=y
```

**Execute QEMU**
``` console
$ qemu-system-x86_64 -smp 2 -m 1024 \
    -nographic \
    -append "console=ttyS0" \
    -kernel ./bin/targets/x86/64/openwrt-x86-64-generic-initramfs-kernel.bin
```


# Reference
[Running 64- and 32-bit RISC-V Linux on QEMU](https://risc-v-getting-started-guide.readthedocs.io/en/latest/linux-qemu.html)



[在QEMU上執行64 bit RISC-V Linux](https://medium.com/swark/%E5%9C%A8qemu%E4%B8%8A%E5%9F%B7%E8%A1%8C64-bit-risc-v-linux-2a527a078819)