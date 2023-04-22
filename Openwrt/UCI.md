---
title: UCI(Unified Configuration Interface)
tags: [OpenWrt]

---

# UCI(Unified Configuration Interface)
###### tags: `OpenWRT`


Different presentation
- Human-friendly: as presented in the config files or with the command “uci export <config>”
- Programmable: as presented with the command “uci show <config>”

#### Configuration files
[Configuration example](https://hackmd.io/ya8BeBw2Qgy_YslcLUl-Jg)

#### C API
>uci_set
uci_commit()
uci_unload()
uci_free_context()
uci_lookup_option()
uci_foreach_element()
load_config()
uci_load()
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
> issue wireless new setting# UCI(Unified Configuration Interface)
###### tags: `OpenWRT`


Different presentation
- Human-friendly: as presented in the config files or with the command “uci export <config>”
- Programmable: as presented with the command “uci show <config>”

#### Configuration files
[Configuration example](https://hackmd.io/ya8BeBw2Qgy_YslcLUl-Jg)

#### C API
>uci_set
uci_commit()
uci_unload()
uci_free_context()
uci_lookup_option()
uci_foreach_element()
load_config()
uci_load()
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