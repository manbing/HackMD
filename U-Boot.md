---
title: U-Boot
tags: [Open Source]

---

# U-Boot

[Das U-boot](https://docs.u-boot.org/en/latest/)

## Legacy uImage

**Create old legacy image:**
``` console
$ mkimage -A powerpc -O linux -T kernel -C gzip \\
-a 0 -e 0 -n Linux -d vmlinux.gz uImage
``` 
## FIT uImage
[u-boot FIT image介绍](http://www.wowotech.net/u-boot/fit_image_overview.html)

**Create FIT image:**
``` console
$ mkimage -f kernel.its kernel.itb
```


## UBI images

[http://www.linux-mtd.infradead.org/doc/ubi.html](http://www.linux-mtd.infradead.org/doc/ubi.html)

UBI user-space tools, as well as other MTD user-space tools, are available from the following git repository:
> http://git.infradead.org/mtd-utils.git

This section provides information about how to compile the whole mtd-utils repository tree. You should find the UBI tools under the ubi-utils sub-directory.

The repository contains the following UBI tools:

* `ubinfo` - provides information about UBI devices and volumes found in the system;
* `ubiattach` - attaches MTD devices (which describe raw flash) with UBI which creates corresponding UBI devices;
* `ubidetach` - detaches MTD devices from UBI devices (the opposite to what ubiattach does);
* `ubimkvol` - creates UBI volumes on UBI devices;
* `ubirmvol` - removes UBI volumes from UBI devices;
* `ubiblock` - manages block interfaces for UBI volumes. See here for more information;
* `ubiupdatevol` - updates UBI volumes; this tool uses the UBI volume update feature which leaves the volume in "corrupted" state if the update was interrupted; additionally, this tool may be used to wipe out UBI volumes;
* `ubicrc32` - calculates CRC-32 checksum of a file with the same initial seed as UBI would use;
* `ubinize` - generates UBI images;
* `ubiformat` - formats empty flash, erases flash and preserves erase counters, flashes UBI images to MTD devices;
* `mtdinfo` - reports information about MTD devices found in the system.


``` console
$ ubinize -m 4096 -p 256KiB -o root.ubi ./flash_partition/tmp-ubinize.cfg
```

## Commands
1. `source [<addr>][:[<image>]|#[<config>]]`

The source command is used to execute a script file from memory.
Two formats for script files exist:
* legacy U-Boot image format
* Flat Image Tree (FIT)

The benefit of the FIT images is that they can be signed and verifed as described in U-Boot FIT Signature Verification.

Both formats can be created with the mkimage tool.


2. `setenv bootargs "<value>"`



3. `saveenv`


## Reference
[mkimage(1) - Linux man page](https://linux.die.net/man/1/mkimage)

**Description**

The mkimage command is used to create images for use with the U-Boot boot loader. Thes eimages can contain the linux kernel, device tree blob, root file system image, firmware images etc., either separate or combined.

**List image information:**
``` console
$ mkimage -l uImage
```


## Case

### Qualcomm
* Generate `u-boot.mbn`

* Generate UBI image, `root.ubi`
include `kernel`, `rootfs`
`kernel` is `openwrt-ipq53xx-ipq53xx_32-qcom_mixx-fit-uImage.itb`
`rootfs` is `openwrt-ipq53xx-ipq53xx_32-squashfs-root.img`

* Generate FIT image, `fitImage.itb`
include `u-boot.mbn`, `root.ubi`# U-Boot

[Das U-boot](https://docs.u-boot.org/en/latest/)

## Legacy uImage

**Create old legacy image:**
``` console
$ mkimage -A powerpc -O linux -T kernel -C gzip \\
-a 0 -e 0 -n Linux -d vmlinux.gz uImage
``` 
## FIT uImage
[u-boot FIT image介绍](http://www.wowotech.net/u-boot/fit_image_overview.html)

**Create FIT image:**
``` console
$ mkimage -f kernel.its kernel.itb
```


## UBI images

[http://www.linux-mtd.infradead.org/doc/ubi.html](http://www.linux-mtd.infradead.org/doc/ubi.html)

UBI user-space tools, as well as other MTD user-space tools, are available from the following git repository:
> http://git.infradead.org/mtd-utils.git

This section provides information about how to compile the whole mtd-utils repository tree. You should find the UBI tools under the ubi-utils sub-directory.

The repository contains the following UBI tools:

* `ubinfo` - provides information about UBI devices and volumes found in the system;
* `ubiattach` - attaches MTD devices (which describe raw flash) with UBI which creates corresponding UBI devices;
* `ubidetach` - detaches MTD devices from UBI devices (the opposite to what ubiattach does);
* `ubimkvol` - creates UBI volumes on UBI devices;
* `ubirmvol` - removes UBI volumes from UBI devices;
* `ubiblock` - manages block interfaces for UBI volumes. See here for more information;
* `ubiupdatevol` - updates UBI volumes; this tool uses the UBI volume update feature which leaves the volume in "corrupted" state if the update was interrupted; additionally, this tool may be used to wipe out UBI volumes;
* `ubicrc32` - calculates CRC-32 checksum of a file with the same initial seed as UBI would use;
* `ubinize` - generates UBI images;
* `ubiformat` - formats empty flash, erases flash and preserves erase counters, flashes UBI images to MTD devices;
* `mtdinfo` - reports information about MTD devices found in the system.


``` console
$ ubinize -m 4096 -p 256KiB -o root.ubi ./flash_partition/tmp-ubinize.cfg
```

## Commands
1. `source [<addr>][:[<image>]|#[<config>]]`

The source command is used to execute a script file from memory.
Two formats for script files exist:
* legacy U-Boot image format
* Flat Image Tree (FIT)

The benefit of the FIT images is that they can be signed and verifed as described in U-Boot FIT Signature Verification.

Both formats can be created with the mkimage tool.


2. `setenv bootargs "<value>"`



3. `saveenv`


## Reference
[mkimage(1) - Linux man page](https://linux.die.net/man/1/mkimage)

**Description**

The mkimage command is used to create images for use with the U-Boot boot loader. Thes eimages can contain the linux kernel, device tree blob, root file system image, firmware images etc., either separate or combined.

**List image information:**
``` console
$ mkimage -l uImage
```


## Case

### Qualcomm
* Generate `u-boot.mbn`

* Generate UBI image, `root.ubi`
include `kernel`, `rootfs`
`kernel` is `openwrt-ipq53xx-ipq53xx_32-qcom_mixx-fit-uImage.itb`
`rootfs` is `openwrt-ipq53xx-ipq53xx_32-squashfs-root.img`

* Generate FIT image, `fitImage.itb`
include `u-boot.mbn`, `root.ubi`