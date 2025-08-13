---
title: UCI (Unified Configuration Interface)
tags: [OpenWrt]

---

Unified Configuration Interface (UCI) is OpenWRT configuration system. UCI configuration consist of two files, package and change delta.
- Package file is saved in configuration directory (locate in flash), `/etc/config`;
- Change delta file is saved in save directory (locate in RAM), `/tmp/.uci`;

Web GUI cache is locate in `/var/run/rpcd/uci-<sha256>`.

``` vim
# rpcd/src/include/rpcd/uci.h
#define RPC_UCI_DIR_PREFIX  "/var/run/rpcd"
#define RPC_UCI_SAVEDIR_PREFIX  RPC_UCI_DIR_PREFIX "/uci-"
#define RPC_SNAPSHOT_FILES  RPC_UCI_DIR_PREFIX "/snapshot-files/"
#define RPC_SNAPSHOT_DELTA  RPC_UCI_DIR_PREFIX "/snapshot-delta/"
#define RPC_UCI_DIR     "/etc/config/"
#define RPC_APPLY_TIMEOUT   60
```


UCI system provide a lot of shell command and API to manipulate configuration files. e.g., `uci_set()`, `uci_commit()`, `uci_save()`, `uci_load()`, `uci get`, `uci show`, `uci changes`, `uci revert` and so on.

UCI is system-level program. Many process will call it simultaneously. It is important to guarantee the synchronization of configuration files; It involves synchronization problem here. However, synchronization is speed's opposite. it needs do some trade-off on that.

`uci_save()` has charge of the chagne delta file. `uci_save()` protect the chagne delta file with `flock()` fine-grained. Therefore, it is synchronization. `uci_commit()` is reponsible for the package file. However, `uci_commit()` has readers-writers problems. It is not synchornization. Therefore, developer should use UCI commit very carefully. If the request is changing setting and take effect, UCI save is enough.

Maybe you have a question for UCI commit. Is it bug? In my opinion, it is design, not bug. OpenWrt is GUI-drive system. After system bootup and initialize, `rpcd` process is the only place does UCI commit. When user does some setting in GUI and click apply button, `rpcd` process will receives notification and does UCI commit. Under this situation, the readers-writers issue will not happen on the package file, because only one process does.

Okay, now we have further question. Why did developers do it? What is the adventage it bring?

# Manual Page
## API
`uci_set()`
Modify UCI setting. Changing configuration setting in process memory.

`uci_save()`
Save configuration from <mark>process memory</mark> to <mark>change delta</mark>.

`uci_commit()`
Save configuration from <mark>process memory</mark> and <mark>change delta</mark>, to <mark>package</mark>.

`uci_load()`
Import configuration from package.

## Shell Command
`uci show`, `uci get`
Display UCI setting. This command includes below flow:
1. Loading configuration file from package to process memory.
2. Loading configruation file from change delta to process memory.
3. Showing complete configration from process memory.

`uci set`
Change UCI and save to change delta.

`uci changes`
Show change delta.

`uci revert`
Remove change delta.

## Examples
```console
$ uci get prplmesh.config.enable
$ uci show netwok
$ uci export netwok

# Find corrupted configs
$ uci validate

# Showing the not-yet-saved modified values
$ uci changes


$ uci set network.lan.ipaddr=192.168.2.2
$ uci commit network

# Change wireless confiuration
$ uci set wireless.wifinet1.disabled=0

# Save wireless configuration to flash
$ uci commit wireless

# Issue wireless new setting
$ /sbin/wifi down
$ /sbin/wifi up
```


# Reference
[Different presentation](https://openwrt.org/docs/guide-user/base-system/uci#different_presentation)
Unified Configuration Interface (UCI) is OpenWRT configuration system. UCI configuration consist of two files, package and change delta.
- Package file is saved in configuration directory (locate in flash), `/etc/config`;
- Change delta file is saved in save directory (locate in RAM), `/tmp/.uci`;

Web GUI cache is locate in `/var/run/rpcd/uci-<sha256>`.

``` vim
# rpcd/src/include/rpcd/uci.h
#define RPC_UCI_DIR_PREFIX  "/var/run/rpcd"
#define RPC_UCI_SAVEDIR_PREFIX  RPC_UCI_DIR_PREFIX "/uci-"
#define RPC_SNAPSHOT_FILES  RPC_UCI_DIR_PREFIX "/snapshot-files/"
#define RPC_SNAPSHOT_DELTA  RPC_UCI_DIR_PREFIX "/snapshot-delta/"
#define RPC_UCI_DIR     "/etc/config/"
#define RPC_APPLY_TIMEOUT   60
```


UCI system provide a lot of shell command and API to manipulate configuration files. e.g., `uci_set()`, `uci_commit()`, `uci_save()`, `uci_load()`, `uci get`, `uci show`, `uci changes`, `uci revert` and so on.

UCI is system-level program. Many process will call it simultaneously. It is important to guarantee the synchronization of configuration files; It involves synchronization problem here. However, synchronization is speed's opposite. it needs do some trade-off on that.

`uci_save()` has charge of the chagne delta file. `uci_save()` protect the chagne delta file with `flock()` fine-grained. Therefore, it is synchronization. `uci_commit()` is reponsible for the package file. However, `uci_commit()` has readers-writers problems. It is not synchornization. Therefore, developer should use UCI commit very carefully. If the request is changing setting and take effect, UCI save is enough.

Maybe you have a question for UCI commit. Is it bug? In my opinion, it is design, not bug. OpenWrt is GUI-drive system. After system bootup and initialize, `rpcd` process is the only place does UCI commit. When user does some setting in GUI and click apply button, `rpcd` process will receives notification and does UCI commit. Under this situation, the readers-writers issue will not happen on the package file, because only one process does.

Okay, now we have further question. Why did developers do it? What is the adventage it bring?

# Manual Page
## API
`uci_set()`
Modify UCI setting. Changing configuration setting in process memory.

`uci_save()`
Save configuration from <mark>process memory</mark> to <mark>change delta</mark>.

`uci_commit()`
Save configuration from <mark>process memory</mark> and <mark>change delta</mark>, to <mark>package</mark>.

`uci_load()`
Import configuration from package.

## Shell Command
`uci show`, `uci get`
Display UCI setting. This command includes below flow:
1. Loading configuration file from package to process memory.
2. Loading configruation file from change delta to process memory.
3. Showing complete configration from process memory.

`uci set`
Change UCI and save to change delta.

`uci changes`
Show change delta.

`uci revert`
Remove change delta.

## Examples
```console
$ uci get prplmesh.config.enable
$ uci show netwok
$ uci export netwok

# Find corrupted configs
$ uci validate

# Showing the not-yet-saved modified values
$ uci changes


$ uci set network.lan.ipaddr=192.168.2.2
$ uci commit network

# Change wireless confiuration
$ uci set wireless.wifinet1.disabled=0

# Save wireless configuration to flash
$ uci commit wireless

# Issue wireless new setting
$ /sbin/wifi down
$ /sbin/wifi up
```


# Reference
[Different presentation](https://openwrt.org/docs/guide-user/base-system/uci#different_presentation)
