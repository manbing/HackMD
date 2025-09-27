[Das U-boot](https://docs.u-boot.org/en/latest/)

# Legacy uImage
# FIT uImage
[u-boot FIT image介绍](http://www.wowotech.net/u-boot/fit_image_overview.html)

# Shell commands
1. `source [<addr>][:[<image>]|#[<config>]]`

The source command is used to execute a script file from memory.
Two formats for script files exist:
* legacy U-Boot image format
* Flat Image Tree (FIT)

The benefit of the FIT images is that they can be signed and verifed as described in U-Boot FIT Signature Verification.

Both formats can be created with the mkimage tool.


2. `setenv bootargs "<value>"`



3. `saveenv`