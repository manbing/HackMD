# OpenWRT
###### tags: `OpenWRT`

## openwrt build system

make package/network/utils/$\lt$pkg$\gt$/compile V=s
make package/luci/{clean,prepare,compile}
make dirclean
make package/$\lt$pkg$\gt$/compile
make package/$\lt$pkg$\gt$/clean
make package/$\lt$pkg$\gt$/prepare QUILT=1
make package/$\lt$pkg$\gt$/refresh

make package/feeds/toolbox/kexec-tools/download
make package/feeds/toolbox/kexec-tools/prepare
make package/feeds/toolbox/kexec-tools/configure
make package/feeds/toolbox/kexec-tools/compile


    
make target/linux/refresh V=s
make target/linux/update V=s
make target/linux/{clean,compile}

rm -rf tmp/
make menuconfig
make kernel_menuconfig
make prereq

### scripts
./scripts/diffconfig.sh > diffconfig
./scripts/feeds search cgi-io
./scripts/feeds install bluez-libs bluez-utils

### config

    
### patch
To add a completely new patch to an existing package example start with preparing the source directory:
> make package/example/{clean,prepare} V=s QUILT=1


To move the new patch file over to the buildroot, run update on the package (buildroot/build_dir -> buildroot/):
> make package/helloworld/update V=s
> make target/linux/update V=s


(buildroot/ -> buildroot/build_dir):
> make target/linux/refresh V=s

[Working with patches](https://openwrt.org/docs/guide-developer/toolchain/use-patches-with-buildsystem#working_with_patches)

### Directory structure
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



### Update kernel version
vi openwrt/include/kernel-version.mk
```
LINUX_VERSION-5.4 = .117
LINUX_KERNEL_HASH-5.4.117 = 4e989b5775830092e5c76b5cca65ebff862ad0c87d0b58c3a20d415c3d4ec770
```
sha256sum

### Decompress .ipk file
mv aaa.ipk aaa.tgz
tar zxvf aaa.tgz

### quilt
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
    
#### create patch
>quilt new 001-main_test.patch
quilt edit tftp/main.c
quilt diff
quilt refresh

[Linux - 管理 patch 的工具 : quilt](http://kaiyhsu.blogspot.com/2018/01/quilt-patch.html)
    
### LuCI WebUI
Collections
Modules
Applications
Themes

### default config
```
mkdir -p files/etc/config
scp root@192.168.1.1:/etc/config/network files/etc/config/
scp root@192.168.1.1:/etc/config/wireless files/etc/config/
scp root@192.168.1.1:/etc/config/firewall files/etc/config/
make image PROFILE="wl500gp" PACKAGES="nano openvpn -ppp -ppp-mod-pppoe" FILES="files"
```
[Using the Image Builder](https://openwrt.org/docs/guide-user/additional-software/imagebuilder)
    
## openwrt system
/etc/init.d/$\lt$config$\gt$ restart
    
factory reset
> umount /overlay && jffs2reset && reboot now
firstboot && reboot now

opkg/ipkg
> opkg install $\lt$pkg$\gt$
opkg list

netifd
> /sbin/netifd -d 15

### procd
#### reload_config
if configuration be modified, it will notify porcd to re-read configuration and take effect

#### procd init script parameters (Init Scripts)
A procd init script is similiar to an old init script, but with a few differences:
* procd expects services to run in the foreground
* Different shebang line: #!/bin/sh /etc/rc.common
* procd expects that shell variable (not environment variable) initscript is set to the path of the script that invoked it
* Explicitly use procd USE_PROCD=1
```
#!/bin/sh /etc/rc.common

USE_PROCD=1
```

### ubus/ubusd
#### shell API
> ubus list
> ubus -v list network.wireless
ubus call network reload

#### C API
> ubus_add_object()
UBUS_OBJECT_TYPE
UBUS_METHOD_NOARG
UBUS_METHOD

### [The UCI system](https://openwrt.org/docs/guide-user/base-system/uci)

Different presentation
- Human-friendly: as presented in the config files or with the command “uci export <config>”
- Programmable: as presented with the command “uci show <config>”

#### Configuration files

Interfaces
> // Bridge
> network.bridge=interface
network.bridge.type='bridge'
network.bridge.ifname='eth0.1' // only for wired LAN inteface
network.bridge.proto='dhcp'
> 
>network.bridge2=interface
network.bridge2.type='bridge'
network.bridge2.ifname='eth0.2'
network.bridge2.proto='static'
network.bridge2.ipaddr='192.168.1.1'
network.bridge2.netmask='255.255.255.0'
>
> network.bridge3=interface
network.bridge3.type='bridge'
network.bridge3.ifname='eth0.3'
network.bridge3.proto='none'

>// Ethernet LAN port
> network.EthLan4=interface
network.EthLan.ifname='eth0.1'
network.EthLan.macaddr='62:11:22:aa:bb:cc'
network.EthLan.disabled='1'

> // Ethernet WAN port
>network.EthWAN=interface
network.EthWAN.ifname='nas0'
network.EthWAN.macaddr='62:11:22:aa:bb:cc'
network.EthWAN.disabled='0'
network.EthWAN.proto='dhcp'  

Wi-Fi
> // Interface
>wireless.wl0=wifi-device
wireless.wifi-device.type='broadcom'
wireless.wifi-device.channel='6'
>
> // inteface attribute
>wireless.@wifi-iface[0]=wifi-iface
wireless.@wifi-iface[0].device='wl0'
wireless.@wifi-iface[0].netwrok='bridge' // section of network.interface
wireless.@wifi-iface[0].mode='ap'
wireless.@wifi-iface[0].ssid='MyWiFiAP' 

DHCP Server
> dhcp.@dhcp[0]=dhcp
dhcp.@dhcp[0].interface='bridge' // section of network.interface
dhcp.@dhcp[0].start='100'
dhcp.@dhcp[0].limit='150'
dhcp.@dhcp[0].leasetime='12h'
    
#### C API
>uci_set
uci_commit()
uci_unload()
uci_free_context()
uci_lookup_option()
uci_foreach_element()
load_config()
uci_lookup_option_string()

#### shell
uci get prplmesh.config.enable
uci show $\lt$SUBSYSTEM_NAME$\gt$
uci export $\lt$SUBSYSTEM_NAME$\gt$

uci validate
> find corrupted configs

uci changes
> Showing the not-yet-saved modified values

uci set network.lan.ipaddr=192.168.2.2
uci commit $\lt$config$\gt$

uci set wireless.wifinet1.disabled=0
> change wireless confiuration

uci commit wireless
> save wireless configuration

/sbin/wifi down
/sbin/wifi up
> issue wireless new setting

### LuCI WebUI


### shell scripts
> json_add_object
> json_add_array

### wireless
>/lib/netifd/wireless/mac80211.sh
/lib/wifi/mac80211.sh

call init_wireless_driver() to add default wireless configuration