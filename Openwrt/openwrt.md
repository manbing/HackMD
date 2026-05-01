---
title: OpenWrt
tags: [OpenWrt]

---

# OpenWrt

``` console
$ /etc/init.d/<config> restart
```
    
factory reset:
``` console
$ umount /overlay ; sleep 1; mtd -r erase rootfs_data
$ firstboot && reboot now
$ fitstboot -r -y
```

opkg/ipkg
``` console
$ opkg install <pkg>
$ opkg list
```

Enable netifd debug:
``` console
$ /sbin/netifd -d 15
```

Extract `odhcp6c` log only from `syslog`:
``` console
$ logread -e odhcp6c -f &
```


If configuration be modified, it will notify porcd to re-read configuration and take effect:
``` console
$ reload_config
```

# procd
## procd init script parameters (Init Scripts)
A procd init script is similiar to an old init script, but with a few differences:
* procd expects services to run in the foreground
* Different shebang line: #!/bin/sh /etc/rc.common
* procd expects that shell variable (not environment variable) initscript is set to the path of the script that invoked it
* Explicitly use procd USE_PROCD=1


``` vim
#!/bin/sh /etc/rc.common

USE_PROCD=1
```

procd_set_param stdout 1
procd_set_param stderr 1

# ubus/ubusd
## shell API
``` console
$ ubus list
$ ubus -v list network.wireless
$ ubus call network reload
$ ubus list network.interface.*

$ ubus list
$ ubus -v list testbed.girl
$ ubus call testbed.girl restart
$ ubus call testbed.girl reload

$ ubus call network.interface.iphost_4001 status | jsonfilter -e '@["ipv4-address"][0].address'
$ ubus call network.interface.iphost_4001 status | jsonfilter -e '@.inactive'
$ ubus call network.interface.iphost_4001 status | jsonfilter -e '@["inactive"]["route"][0].nexthop'
$ ubus call network.device status  '{"name":"pon.4001"}'
```

shell scripts:
``` vim
json_add_object
json_add_array
```

## C API
``` c
ubus_add_object()
UBUS_OBJECT_TYPE
UBUS_METHOD_NOARG
UBUS_METHOD

ubus_invoke()
```

Ubus call the RPC of "uci commit":
``` c
static inline void example(const char *package)
{
    uint32_t id = 0;
    struct blob_buf b = {NULL, NULL, 0, NULL};
    struct ubus_context *ubus_ctx = ubus_connect(NULL);

    if (ubus_lookup_id(ubus_ctx, "uci", &id) != UBUS_STATUS_OK) {
        ubus_free(ubus_ctx);
        return;
    }

    blob_buf_init(&b, 0);
    blobmsg_add_string(&b, "config", package);
    ubus_invoke(ubus_ctx, id, "commit", b.head, NULL, 0, 1000);
    ubus_free(ubus_ctx);
}
```


# [The UCI system](https://openwrt.org/docs/guide-user/base-system/uci)

Different presentation
- Human-friendly: as presented in the config files or with the command “uci export <config>”
- Programmable: as presented with the command “uci show <config>”

    
## Package/Configuration files
[Configuration example](https://hackmd.io/ya8BeBw2Qgy_YslcLUl-Jg)

### [Network](https://openwrt.org/docs/guide-user/network/ucicheatsheet)
#### [IPv6](https://openwrt.org/docs/guide-user/network/ipv6/configuration)
``` vim
network.lan=interface
network.lan.ip6assign='auto'
network.lan.ipv6='1'

# Define the IPv6 prefix-classes this interface will accept
network.lan.ip6class='HSI_6'
```

### [wireless](https://openwrt.org/docs/guide-user/network/wifi/basic)
    
    
## C API
``` c
uci_set()
uci_commit()
uci_unload()
uci_free_context()
uci_lookup_option()
uci_foreach_element()
load_config()
uci_load()
uci_lookup_option_string()
```

`uci_lookup_ptr()`
``` c
    struct uci_ptr ptr = {
        .package = "wireless",
        .section = name,
        .flags = (name && *name == '@') ? UCI_LOOKUP_EXTENDED : 0,
    };
    const char *opt;

    if (!uci_ctx) {
        uci_ctx = uci_alloc_context();
        if (!uci_ctx)
            return NULL;
    }

    if (uci_lookup_ptr(uci_ctx, &ptr, NULL, true))
        return NULL;

    if (!ptr.s || strcmp(ptr.s->type, "wifi-device") != 0)
        return NULL;

    opt = uci_lookup_option_string(uci_ctx, ptr.s, "type");
    if (!opt || strcmp(opt, type) != 0)
        return NULL;

    return ptr.s;

```
    
    

## SHELL
``` console
$ uci get prplmesh.config.enable
$ uci show <SUBSYSTEM_NAME>
$ uci export <SUBSYSTEM_NAME>
```

Do not use extended syntax on 'show' for unnamed section:
``` console
$ uci show -X network
network.4001_ipoe_3_1=wan_vlan
network.4001_ipoe_3_1.payload='routed'
network.4001_ipoe_3_1.type='ipoe'
network.cfg140f15=device
network.cfg140f15.type='bridge'
```

Find corrupted configs:
``` console
$ uci validate
```

Showing the not-yet-saved modified values:
``` console
$ uci changes
```  

``` console
$ uci set network.lan.ipaddr=192.168.2.2
$ uci commit <config>
```

Change wireless confiuration:
``` console
$ uci set wireless.wifinet1.disabled=0
```

Save wireless configuration:
``` console
$ uci commit wireless
```

Issue wireless new setting:
``` console
$ /sbin/wifi down
$ /sbin/wifi up
```


## LuCI WebUI


## wireless
Call init_wireless_driver() to add default wireless configuration:
``` console
$ /lib/netifd/wireless/mac80211.sh
$ /lib/wifi/mac80211.sh
```
    
## ucitrack
    
    
## [UCI defaults](https://openwrt.org/docs/guide-developer/uci-defaults)
OpenWrt relies on UCI, the Unified Configuration Interface, to configure its core services. UCI defaults provides a way to preconfigure your images, using UCI.

To set some system defaults the first time the device boots, create a script in the directory `/etc/uci-defaults`.

All scripts in that directory are automatically executed by the `boot` service:

* If they exit with code 0 they are deleted afterwards.
* Scripts that exit with non-zero exit code are not deleted and will be re-executed at the next boot until they also successfully exit.
In a live router you can see the existing UCI defaults scripts in `/rom/etc/uci-defaults`, as `/etc/uci-defaults` itself is typically empty (after all scripts have been run successfully and have been deleted).

UCI defaults scripts can be created by packages or they can be inserted into the build manually as custom files.
    
# The boot process
    
``` mermaid
    flowchart LR;
    
    A((linux))-->B(init)
    B-->C([/etc/preinit])
    B-->D(procd)
    C-->E([/lib/preinit/])
    D-->F([/etc/rc.d/])
    F-->G([S10boot])
    F-->H([S20network])
    E-->I([00_preinit.conf])
    G-->J([/etc/uci-defaults/])
```
    
# shell
``` sh
network_get_dnsserver
procd_set_param stdout 1
procd_set_param stderr 1
```
    
# Console login
If `system.@system[0].ttylogin` is 1, system will request password to authenticate. If it is not, system gives root privilige.

`/usr/libexec/login.sh`:
``` sh
#!/bin/sh

[ "$(uci -q get system.@system[0].ttylogin)" = 1 ] || exec /bin/ash --login

exec /bin/login
```# OpenWrt

``` console
$ /etc/init.d/<config> restart
```
    
factory reset:
``` console
$ umount /overlay ; sleep 1; mtd -r erase rootfs_data
$ firstboot && reboot now
$ fitstboot -r -y
```

opkg/ipkg
``` console
$ opkg install <pkg>
$ opkg list
```

Enable netifd debug:
``` console
$ /sbin/netifd -d 15
```

Extract `odhcp6c` log only from `syslog`:
``` console
$ logread -e odhcp6c -f &
```


If configuration be modified, it will notify porcd to re-read configuration and take effect:
``` console
$ reload_config
```

# procd
## procd init script parameters (Init Scripts)
A procd init script is similiar to an old init script, but with a few differences:
* procd expects services to run in the foreground
* Different shebang line: #!/bin/sh /etc/rc.common
* procd expects that shell variable (not environment variable) initscript is set to the path of the script that invoked it
* Explicitly use procd USE_PROCD=1


``` vim
#!/bin/sh /etc/rc.common

USE_PROCD=1
```

procd_set_param stdout 1
procd_set_param stderr 1

# ubus/ubusd
## shell API
``` console
$ ubus list
$ ubus -v list network.wireless
$ ubus call network reload
$ ubus list network.interface.*

$ ubus list
$ ubus -v list testbed.girl
$ ubus call testbed.girl restart
$ ubus call testbed.girl reload

$ ubus call network.interface.iphost_4001 status | jsonfilter -e '@["ipv4-address"][0].address'
$ ubus call network.interface.iphost_4001 status | jsonfilter -e '@.inactive'
$ ubus call network.interface.iphost_4001 status | jsonfilter -e '@["inactive"]["route"][0].nexthop'
$ ubus call network.device status  '{"name":"pon.4001"}'
```

shell scripts:
``` vim
json_add_object
json_add_array
```

## C API
``` c
ubus_add_object()
UBUS_OBJECT_TYPE
UBUS_METHOD_NOARG
UBUS_METHOD

ubus_invoke()
```

Ubus call the RPC of "uci commit":
``` c
static inline void example(const char *package)
{
    uint32_t id = 0;
    struct blob_buf b = {NULL, NULL, 0, NULL};
    struct ubus_context *ubus_ctx = ubus_connect(NULL);

    if (ubus_lookup_id(ubus_ctx, "uci", &id) != UBUS_STATUS_OK) {
        ubus_free(ubus_ctx);
        return;
    }

    blob_buf_init(&b, 0);
    blobmsg_add_string(&b, "config", package);
    ubus_invoke(ubus_ctx, id, "commit", b.head, NULL, 0, 1000);
    ubus_free(ubus_ctx);
}
```


# [The UCI system](https://openwrt.org/docs/guide-user/base-system/uci)

Different presentation
- Human-friendly: as presented in the config files or with the command “uci export <config>”
- Programmable: as presented with the command “uci show <config>”

    
## Package/Configuration files
[Configuration example](https://hackmd.io/ya8BeBw2Qgy_YslcLUl-Jg)

### [Network](https://openwrt.org/docs/guide-user/network/ucicheatsheet)
#### [IPv6](https://openwrt.org/docs/guide-user/network/ipv6/configuration)
``` vim
network.lan=interface
network.lan.ip6assign='auto'
network.lan.ipv6='1'

# Define the IPv6 prefix-classes this interface will accept
network.lan.ip6class='HSI_6'
```

### [wireless](https://openwrt.org/docs/guide-user/network/wifi/basic)
    
    
## C API
``` c
uci_set()
uci_commit()
uci_unload()
uci_free_context()
uci_lookup_option()
uci_foreach_element()
load_config()
uci_load()
uci_lookup_option_string()
```

`uci_lookup_ptr()`
``` c
    struct uci_ptr ptr = {
        .package = "wireless",
        .section = name,
        .flags = (name && *name == '@') ? UCI_LOOKUP_EXTENDED : 0,
    };
    const char *opt;

    if (!uci_ctx) {
        uci_ctx = uci_alloc_context();
        if (!uci_ctx)
            return NULL;
    }

    if (uci_lookup_ptr(uci_ctx, &ptr, NULL, true))
        return NULL;

    if (!ptr.s || strcmp(ptr.s->type, "wifi-device") != 0)
        return NULL;

    opt = uci_lookup_option_string(uci_ctx, ptr.s, "type");
    if (!opt || strcmp(opt, type) != 0)
        return NULL;

    return ptr.s;

```
    
    

## SHELL
``` console
$ uci get prplmesh.config.enable
$ uci show <SUBSYSTEM_NAME>
$ uci export <SUBSYSTEM_NAME>
```

Do not use extended syntax on 'show' for unnamed section:
``` console
$ uci show -X network
network.4001_ipoe_3_1=wan_vlan
network.4001_ipoe_3_1.payload='routed'
network.4001_ipoe_3_1.type='ipoe'
network.cfg140f15=device
network.cfg140f15.type='bridge'
```

Find corrupted configs:
``` console
$ uci validate
```

Showing the not-yet-saved modified values:
``` console
$ uci changes
```  

``` console
$ uci set network.lan.ipaddr=192.168.2.2
$ uci commit <config>
```

Change wireless confiuration:
``` console
$ uci set wireless.wifinet1.disabled=0
```

Save wireless configuration:
``` console
$ uci commit wireless
```

Issue wireless new setting:
``` console
$ /sbin/wifi down
$ /sbin/wifi up
```


## LuCI WebUI


## wireless
Call init_wireless_driver() to add default wireless configuration:
``` console
$ /lib/netifd/wireless/mac80211.sh
$ /lib/wifi/mac80211.sh
```
    
## ucitrack
    
    
## [UCI defaults](https://openwrt.org/docs/guide-developer/uci-defaults)
OpenWrt relies on UCI, the Unified Configuration Interface, to configure its core services. UCI defaults provides a way to preconfigure your images, using UCI.

To set some system defaults the first time the device boots, create a script in the directory `/etc/uci-defaults`.

All scripts in that directory are automatically executed by the `boot` service:

* If they exit with code 0 they are deleted afterwards.
* Scripts that exit with non-zero exit code are not deleted and will be re-executed at the next boot until they also successfully exit.
In a live router you can see the existing UCI defaults scripts in `/rom/etc/uci-defaults`, as `/etc/uci-defaults` itself is typically empty (after all scripts have been run successfully and have been deleted).

UCI defaults scripts can be created by packages or they can be inserted into the build manually as custom files.
    
# The boot process
    
``` mermaid
    flowchart LR;
    
    A((linux))-->B(init)
    B-->C([/etc/preinit])
    B-->D(procd)
    C-->E([/lib/preinit/])
    D-->F([/etc/rc.d/])
    F-->G([S10boot])
    F-->H([S20network])
    E-->I([00_preinit.conf])
    G-->J([/etc/uci-defaults/])
```
    
# shell
``` sh
network_get_dnsserver
procd_set_param stdout 1
procd_set_param stderr 1
```
    
# Console login
If `system.@system[0].ttylogin` is 1, system will request password to authenticate. If it is not, system gives root privilige.

`/usr/libexec/login.sh`:
``` sh
#!/bin/sh

[ "$(uci -q get system.@system[0].ttylogin)" = 1 ] || exec /bin/ash --login

exec /bin/login
```