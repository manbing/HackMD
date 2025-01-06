# OpenWrt build system

```
& mkdir -p files/etc/config
& scp root@192.168.1.1:/etc/config/network files/etc/config/
& scp root@192.168.1.1:/etc/config/wireless files/etc/config/
& scp root@192.168.1.1:/etc/config/firewall files/etc/config/
& make image PROFILE="wl500gp" PACKAGES="nano openvpn -ppp -ppp-mod-pppoe" FILES="files"
```

## Make
```
$ make package/network/utils/$\lt$pkg$\gt$/compile V=s
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

```
$ make target/linux/refresh V=s
$ make target/linux/update V=s
$ make target/linux/{clean,compile}
```

```
$ make toolchain/install
$ make toolchain/{clean,prepare}
$ make toolchain/{clean,compile}
```


re-generate .config:
```
$ rm -rf tmp/
$ make menuconfig
$ make kernel_menuconfig
$ make prereq
```


### [Cleaning up](https://openwrt.org/docs/guide-developer/toolchain/use-buildsystem#cleaning_up)
make dirclean
make clean
make targetclean

## scripts
./scripts/diffconfig.sh > diffconfig
./scripts/feeds search cgi-io
./scripts/feeds install bluez-libs bluez-utils

## config

    
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
quilt series: 查詢目前有哪些 patch
quilt applied: 查詢已經 apply 的 patch
quilt unapplied: 查詢還沒 apply 的 patch
quilt next:
quilt top:
quilt refresh:
quilt push
quilt pop
quilt new 115-new-test.patch
quilt add quilt_add_01.c
quilt edit quilt_add_02.c
quilt remove quilt_add_01.c
quilt refresh
quilt files
> Print the list of files that the topmost or specified patch changes.
    
### create patch
>quilt new 001-main_test.patch
quilt edit tftp/main.c
quilt diff
quilt refresh

[Linux - 管理 patch 的工具 : quilt](http://kaiyhsu.blogspot.com/2018/01/quilt-patch.html)
    
## LuCI WebUI
Collections
Modules
Applications
Themes


[Using the Image Builder](https://openwrt.org/docs/guide-user/additional-software/imagebuilder)
  