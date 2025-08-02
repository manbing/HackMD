---
title: OpenWrt build system
tags: [OpenWrt]

---

# OpenWrt build system

## Quick Start
``` shell
$ ./scripts/feeds update -a
$ ./scripts/feeds install -a
$ make menuconfig
$ make kernel_menuconfig
$ make V=s -j$(nproc)
```

``` shell
$ mkdir -p files/etc/config
$ scp root@192.168.1.1:/etc/config/network files/etc/config/
$ scp root@192.168.1.1:/etc/config/wireless files/etc/config/
$ scp root@192.168.1.1:/etc/config/firewall files/etc/config/
$ make image PROFILE="wl500gp" PACKAGES="nano openvpn -ppp -ppp-mod-pppoe" FILES="files"
```

## Make
`*` in menuconfig means build and include the package in firmware ('=y' in .config)
`M` means just build the package as an installable `*.ipk` but do NOT include in firmware ('=m' in .config)

``` console
$ make menuconfig
$ make kernel_menuconfig
```


```  shell
$ make package/network/utils/<pkg>/compile V=s
$ make package/luci/{clean,prepare,compile}

$ make package/<pkg>/compile
$ make package/<pkg>/clean
$ make package/<pkg>/prepare QUILT=1
$ make package/<pkg>/refresh

$ make package/feeds/toolbox/kexec-tools/download
$ make package/feeds/toolbox/kexec-tools/prepare
$ make package/feeds/toolbox/kexec-tools/configure
$ make package/feeds/toolbox/kexec-tools/compile
```

```  shell
$ make target/linux/refresh V=s
$ make target/linux/update V=s
$ make target/linux/{clean,compile}
```

```  shell
$ make toolchain/install
$ make toolchain/{clean,prepare}
$ make toolchain/{clean,compile}
```


### [Cleaning up](https://openwrt.org/docs/guide-developer/toolchain/use-buildsystem#cleaning_up)
```  shell
make dirclean
make clean
make targetclean
```

## scripts
``` shell
$ ./scripts/diffconfig.sh > diffconfig
$ ./scripts/feeds search cgi-io
$ ./scripts/feeds install bluez-libs bluez-utils
$ ./scripts/feeds update -a
$ ./scripts/feeds install -a
```

## config
$ CONFIG_NO_STRIP=y
> Select the binary stripping method you wish to use.
Prompt: Binary stripping method
Location:
-> Global build settings                                             

Re-generate .config:
```  shell
$ rm -rf tmp/
$ make menuconfig
$ make kernel_menuconfig
$ make prereq
```

Create .config from diff configuration:
``` console
$ cp diffconfig .config
$ make defconfig
```
 
 Create diff configuration:
 ``` console
 $ scripts/diffconfig.sh > diffconfig
 ```
 
## patch
To add a completely new patch to an existing package example start with preparing the source directory:
> make package/example/{clean,prepare} V=s QUILT=1


To move the new patch file over to the buildroot, run update on the package (buildroot/build_dir -> buildroot/):
> make package/helloworld/update V=s
> make target/linux/update V=s


(buildroot/ -> buildroot/build_dir):
> make target/linux/refresh V=s

[Working with patches](https://openwrt.org/docs/guide-developer/toolchain/use-patches-with-buildsystem#working_with_patches)

## Directory structure
rootfs:
>openwrt/staging_dir/target-aarch64_cortex-a72_musl/root-bcm27xx

kernel source/output:
>openwrt/build_dir/target-aarch64_cortex-a72_musl/linux-bcm27xx_bcm2711/linux-5.4.117

image:
>openwrt/bin/targets/bcm27xx/bcm2711/openwrt-bcm27xx-bcm2711-rpi-4-squashfs-factory.img.gz

sysroot:
>openwrt/staging_dir/toolchain-aarch64_cortex-a72_gcc-8.4.0_musl

binutils:

>openwrt/build_dir/toolchain-aarch64_cortex-a72_gcc-8.4.0_musl/binutils-2.34/binutils/objdump

>./staging_dir/toolchain-aarch64_cortex-a72_gcc-8.4.0_musl/aarch64-openwrt-linux-musl/bin/objdump


else:
>openwrt/build_dir/toolchain-aarch64_cortex-a72_gcc-8.4.0_musl/gdb-10.1/gdb/gdb



## Update kernel version
vi openwrt/include/kernel-version.mk
```
LINUX_VERSION-5.4 = .117
LINUX_KERNEL_HASH-5.4.117 = 4e989b5775830092e5c76b5cca65ebff862ad0c87d0b58c3a20d415c3d4ec770
```
sha256sum

## Decompress .ipk file
mv aaa.ipk aaa.tgz
tar zxvf aaa.tgz

## quilt
``` shell
#查詢目前有哪些 patch
$ quilt series

#查詢已經 apply 的 patch
$ quilt applied

#查詢還沒 apply 的 patch
$ quilt unapplied
$ quilt next
$ quilt top
$ quilt refresh
$ quilt push -a
$ quilt pop
$ quilt new 115-new-test.patch
$ quilt add quilt_add_01.c
$ quilt edit quilt_add_02.c
$ quilt remove quilt_add_01.c
$ quilt refresh

#Print the list of files that the topmost or specified patch changes.
$ quilt files
```
    
### create patch
``` shell
$ make path/to/package/{clean,prepare} V=s QUILT=1
$ quilt new 001-main_test.patch
$ quilt edit path/to/file.c
$ quilt diff
$ quilt refresh
$ make path/to/package/update V=s -j1
```

[Linux - 管理 patch 的工具 : quilt](http://kaiyhsu.blogspot.com/2018/01/quilt-patch.html)
    
## LuCI WebUI
Collections
Modules
Applications
Themes


[Using the Image Builder](https://openwrt.org/docs/guide-user/additional-software/imagebuilder)
  # OpenWrt build system

## Quick Start
``` shell
$ ./scripts/feeds update -a
$ ./scripts/feeds install -a
$ make menuconfig
$ make kernel_menuconfig
$ make V=s -j$(nproc)
```

``` shell
$ mkdir -p files/etc/config
$ scp root@192.168.1.1:/etc/config/network files/etc/config/
$ scp root@192.168.1.1:/etc/config/wireless files/etc/config/
$ scp root@192.168.1.1:/etc/config/firewall files/etc/config/
$ make image PROFILE="wl500gp" PACKAGES="nano openvpn -ppp -ppp-mod-pppoe" FILES="files"
```

## Make
`*` in menuconfig means build and include the package in firmware ('=y' in .config)
`M` means just build the package as an installable `*.ipk` but do NOT include in firmware ('=m' in .config)

``` console
$ make menuconfig
$ make kernel_menuconfig
```


```  shell
$ make package/network/utils/<pkg>/compile V=s
$ make package/luci/{clean,prepare,compile}

$ make package/<pkg>/compile
$ make package/<pkg>/clean
$ make package/<pkg>/prepare QUILT=1
$ make package/<pkg>/refresh

$ make package/feeds/toolbox/kexec-tools/download
$ make package/feeds/toolbox/kexec-tools/prepare
$ make package/feeds/toolbox/kexec-tools/configure
$ make package/feeds/toolbox/kexec-tools/compile
```

```  shell
$ make target/linux/refresh V=s
$ make target/linux/update V=s
$ make target/linux/{clean,compile}
```

```  shell
$ make toolchain/install
$ make toolchain/{clean,prepare}
$ make toolchain/{clean,compile}
```


### [Cleaning up](https://openwrt.org/docs/guide-developer/toolchain/use-buildsystem#cleaning_up)
```  shell
make dirclean
make clean
make targetclean
```

## scripts
``` shell
$ ./scripts/diffconfig.sh > diffconfig
$ ./scripts/feeds search cgi-io
$ ./scripts/feeds install bluez-libs bluez-utils
$ ./scripts/feeds update -a
$ ./scripts/feeds install -a
```

## config
$ CONFIG_NO_STRIP=y
> Select the binary stripping method you wish to use.
Prompt: Binary stripping method
Location:
-> Global build settings                                             

Re-generate .config:
```  shell
$ rm -rf tmp/
$ make menuconfig
$ make kernel_menuconfig
$ make prereq
```

Create .config from diff configuration:
``` console
$ cp diffconfig .config
$ make defconfig
```
 
 Create diff configuration:
 ``` console
 $ scripts/diffconfig.sh > diffconfig
 ```
 
## patch
To add a completely new patch to an existing package example start with preparing the source directory:
> make package/example/{clean,prepare} V=s QUILT=1


To move the new patch file over to the buildroot, run update on the package (buildroot/build_dir -> buildroot/):
> make package/helloworld/update V=s
> make target/linux/update V=s


(buildroot/ -> buildroot/build_dir):
> make target/linux/refresh V=s

[Working with patches](https://openwrt.org/docs/guide-developer/toolchain/use-patches-with-buildsystem#working_with_patches)

## Directory structure
rootfs:
>openwrt/staging_dir/target-aarch64_cortex-a72_musl/root-bcm27xx

kernel source/output:
>openwrt/build_dir/target-aarch64_cortex-a72_musl/linux-bcm27xx_bcm2711/linux-5.4.117

image:
>openwrt/bin/targets/bcm27xx/bcm2711/openwrt-bcm27xx-bcm2711-rpi-4-squashfs-factory.img.gz

sysroot:
>openwrt/staging_dir/toolchain-aarch64_cortex-a72_gcc-8.4.0_musl

binutils:

>openwrt/build_dir/toolchain-aarch64_cortex-a72_gcc-8.4.0_musl/binutils-2.34/binutils/objdump

>./staging_dir/toolchain-aarch64_cortex-a72_gcc-8.4.0_musl/aarch64-openwrt-linux-musl/bin/objdump


else:
>openwrt/build_dir/toolchain-aarch64_cortex-a72_gcc-8.4.0_musl/gdb-10.1/gdb/gdb



## Update kernel version
vi openwrt/include/kernel-version.mk
```
LINUX_VERSION-5.4 = .117
LINUX_KERNEL_HASH-5.4.117 = 4e989b5775830092e5c76b5cca65ebff862ad0c87d0b58c3a20d415c3d4ec770
```
sha256sum

## Decompress .ipk file
mv aaa.ipk aaa.tgz
tar zxvf aaa.tgz

## quilt
``` shell
#查詢目前有哪些 patch
$ quilt series

#查詢已經 apply 的 patch
$ quilt applied

#查詢還沒 apply 的 patch
$ quilt unapplied
$ quilt next
$ quilt top
$ quilt refresh
$ quilt push -a
$ quilt pop
$ quilt new 115-new-test.patch
$ quilt add quilt_add_01.c
$ quilt edit quilt_add_02.c
$ quilt remove quilt_add_01.c
$ quilt refresh

#Print the list of files that the topmost or specified patch changes.
$ quilt files
```
    
### create patch
``` shell
$ make path/to/package/{clean,prepare} V=s QUILT=1
$ quilt new 001-main_test.patch
$ quilt edit path/to/file.c
$ quilt diff
$ quilt refresh
$ make path/to/package/update V=s -j1
```

[Linux - 管理 patch 的工具 : quilt](http://kaiyhsu.blogspot.com/2018/01/quilt-patch.html)
    
## LuCI WebUI
Collections
Modules
Applications
Themes


[Using the Image Builder](https://openwrt.org/docs/guide-user/additional-software/imagebuilder)
  