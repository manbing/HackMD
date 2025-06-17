---
title: UCI(Unified Configuration Interface)
tags: [OpenWRT]

---

# UCI(Unified Configuration Interface)
###### tags: `OpenWRT`
UCI is OpenWRT configuration system. UCI configuration consist of two files, package and change delta.
Package is saved in configuration directory (locate in flash), `/etc/config`;
Change delta is saved in save directory (locate in RAM), `/tmp/.uci`.

UCI system provide a lot of shell command and API to manipulate configuration. e.g., `uci_set()`, `uci_commit()`, `uci_save()`, `uci_load()`, `uci get`, `uci show`, `uci changes`, `uci revert` and so on.

UCI is system-level program. Many process will call it simultaneously. It is important to guarantee the synchronization of configuration files; It involves synchronization problem here. However, synchronization is speed's opposite. it needs do some trade-off on that.

In UCI design concept, it provides two save mechanism. one, change delta, is synchronization, it be protected by `flock()` fine-grained; the other one, package, is not synchornization, it has readers-writers problems. Therefore, developer should use the commit function very carefully. If the request is changing setting and take effect, the save function is enough.

## Manual Page
### API
`uci_set()`
Modify UCI setting. Changing configuration setting in process memory.

`uci_save()`
Save configuration from <mark>process memory</mark> to <mark>change delta</mark>.

`uci_commit()`
Save configuration from <mark>process memory</mark> and <mark>change delta</mark>, to <mark>package</mark>.

`uci_load()`
Import configuration from package.

### Shell Command
`uci show`, `uci get`
Display UCI setting. This command includes below flow:
1. Loading configuration file from package to process memory.
2. Loading configruation file from cahnge delta to process memory.
3. Showing complete configration.

`uci set`
Change UCI and save to change delta.

`uci changes`
Show change delta.

`uci revert`
Remove change delta.

### Examples
```
$ uci get prplmesh.config.enable
$ uci show netwok
$ uci export netwok

#Find corrupted configs
$ uci validate

#Showing the not-yet-saved modified values
$ uci changes


$ uci set network.lan.ipaddr=192.168.2.2
$ uci commit network

#Change wireless confiuration
$ uci set wireless.wifinet1.disabled=0

#Save wireless configuration to flash
$ uci commit wireless

#Issue wireless new setting
$ /sbin/wifi down
$ /sbin/wifi up
```


## Reference
[Different presentation](https://openwrt.org/docs/guide-user/base-system/uci#different_presentation)
# UCI(Unified Configuration Interface)
###### tags: `OpenWRT`
UCI is OpenWRT configuration system. UCI configuration consist of two files, package and change delta.
Package is saved in configuration directory (locate in flash), `/etc/config`;
Change delta is saved in save directory (locate in RAM), `/tmp/.uci`.

UCI system provide a lot of shell command and API to manipulate configuration. e.g., `uci_set()`, `uci_commit()`, `uci_save()`, `uci_load()`, `uci get`, `uci show`, `uci changes`, `uci revert` and so on.

UCI is system-level program. Many process will call it simultaneously. It is important to guarantee the synchronization of configuration files; It involves synchronization problem here. However, synchronization is speed's opposite. it needs do some trade-off on that.

In UCI design concept, it provides two save mechanism. one, change delta, is synchronization, it be protected by `flock()` fine-grained; the other one, package, is not synchornization, it has readers-writers problems. Therefore, developer should use the commit function very carefully. If the request is changing setting and take effect, the save function is enough.

## Manual Page
### API
`uci_set()`
Modify UCI setting. Changing configuration setting in process memory.

`uci_save()`
Save configuration from <mark>process memory</mark> to <mark>change delta</mark>.

`uci_commit()`
Save configuration from <mark>process memory</mark> and <mark>change delta</mark>, to <mark>package</mark>.

`uci_load()`
Import configuration from package.

### Shell Command
`uci show`, `uci get`
Display UCI setting. This command includes below flow:
1. Loading configuration file from package to process memory.
2. Loading configruation file from cahnge delta to process memory.
3. Showing complete configration.

`uci set`
Change UCI and save to change delta.

`uci changes`
Show change delta.

`uci revert`
Remove change delta.

### Examples
```
$ uci get prplmesh.config.enable
$ uci show netwok
$ uci export netwok

#Find corrupted configs
$ uci validate

#Showing the not-yet-saved modified values
$ uci changes


$ uci set network.lan.ipaddr=192.168.2.2
$ uci commit network

#Change wireless confiuration
$ uci set wireless.wifinet1.disabled=0

#Save wireless configuration to flash
$ uci commit wireless

#Issue wireless new setting
$ /sbin/wifi down
$ /sbin/wifi up
```


## Reference
[Different presentation](https://openwrt.org/docs/guide-user/base-system/uci#different_presentation)
