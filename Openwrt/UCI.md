---
title: Unified Configuration Interface (UCI)
tags: [OpenWrt]

---

# Unified Configuration Interface (UCI)
# Description
Unified Configuration Interface (UCI) is OpenWRT configuration system. UCI configuration consist of two files, package and change delta.
- <mark>Package file</mark>
In default, it is saved in configuration directory (locate in flash), `/etc/config/`.
- <mark>Change delta file</mark>
In default, it is saved in save directory (locate in RAM), `/tmp/.uci/`.
On Web GUI, RPCD save change delta in `/var/run/rpcd/uci-<sha256>/`.

``` c
/* uci-2023-08-10-5781664d/uci.h */
#define UCI_CONFDIR "/etc/config"
#define UCI_SAVEDIR "/tmp/.uci"
#define UCI_DIRMODE 0700
#define UCI_FILEMODE 0600
```

``` c
/* rpcd/src/include/rpcd/uci.h */
#define RPC_UCI_DIR_PREFIX  "/var/run/rpcd"
#define RPC_UCI_SAVEDIR_PREFIX  RPC_UCI_DIR_PREFIX "/uci-"
#define RPC_SNAPSHOT_FILES  RPC_UCI_DIR_PREFIX "/snapshot-files/"
#define RPC_SNAPSHOT_DELTA  RPC_UCI_DIR_PREFIX "/snapshot-delta/"
#define RPC_UCI_DIR     "/etc/config/"
#define RPC_APPLY_TIMEOUT   60
```

# Manual Page
UCI system provide a lot of shell command and API to manipulate configuration files. e.g., `uci_set()`, `uci_commit()`, `uci_save()`, `uci_load()`, `uci get`, `uci show`, `uci changes`, `uci revert` and so on.

## API
### `uci_import()`
Load package file from flash into process memory.

### `uci_load_delta`
Load delta file (tmpfs) into process memory.

### `uci_load()`
`uci_import()` + `uci_load_delta`

### `uci_commit()`
Dpend on `overwrite`, it will reload package before commit value. Because the package maybe be modified by others process.
``` c
/**
 * uci_commit: commit changes to a package
 * @ctx: uci context
 * @p: uci_package struct pointer
 * @overwrite: overwrite existing config data and flush delta
 *
 * committing may reload the whole uci_package data,
 * the supplied pointer is updated accordingly
 */
extern int uci_commit(struct uci_context *ctx, struct uci_package **p, bool overwrite);
```

`overwrite` is 0:
The event sequence:
1. Chagned delta (Process)
2. Save chagned delta (Process) to tmpfs
3. Load existing package from flash
4. Load chagned delta (tmpfs) to Process
5. Write package to flash
---
if `overwrite` is 1:
1. Chagned delta (Process)
2. Load chagned delta (tmpfs) to Process
3. Write package to flash

### `uci_save()`
Save change delta from <mark>process memory</mark> to <mark>tmpfs</mark>.
``` c
/**
 * uci_save: save change delta for a package
 * @ctx: uci context
 * @p: uci_package struct
 */
extern int uci_save(struct uci_context *ctx, struct uci_package *p);

```

### `uci_set()`
Modify UCI setting. Changing configuration setting in process memory.

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
# Unified Configuration Interface (UCI)
# Description
Unified Configuration Interface (UCI) is OpenWRT configuration system. UCI configuration consist of two files, package and change delta.
- <mark>Package file</mark>
In default, it is saved in configuration directory (locate in flash), `/etc/config/`.
- <mark>Change delta file</mark>
In default, it is saved in save directory (locate in RAM), `/tmp/.uci/`.
On Web GUI, RPCD save change delta in `/var/run/rpcd/uci-<sha256>/`.

``` c
/* uci-2023-08-10-5781664d/uci.h */
#define UCI_CONFDIR "/etc/config"
#define UCI_SAVEDIR "/tmp/.uci"
#define UCI_DIRMODE 0700
#define UCI_FILEMODE 0600
```

``` c
/* rpcd/src/include/rpcd/uci.h */
#define RPC_UCI_DIR_PREFIX  "/var/run/rpcd"
#define RPC_UCI_SAVEDIR_PREFIX  RPC_UCI_DIR_PREFIX "/uci-"
#define RPC_SNAPSHOT_FILES  RPC_UCI_DIR_PREFIX "/snapshot-files/"
#define RPC_SNAPSHOT_DELTA  RPC_UCI_DIR_PREFIX "/snapshot-delta/"
#define RPC_UCI_DIR     "/etc/config/"
#define RPC_APPLY_TIMEOUT   60
```

# Manual Page
UCI system provide a lot of shell command and API to manipulate configuration files. e.g., `uci_set()`, `uci_commit()`, `uci_save()`, `uci_load()`, `uci get`, `uci show`, `uci changes`, `uci revert` and so on.

## API
### `uci_import()`
Load package file from flash into process memory.

### `uci_load_delta`
Load delta file (tmpfs) into process memory.

### `uci_load()`
`uci_import()` + `uci_load_delta`

### `uci_commit()`
Dpend on `overwrite`, it will reload package before commit value. Because the package maybe be modified by others process.
``` c
/**
 * uci_commit: commit changes to a package
 * @ctx: uci context
 * @p: uci_package struct pointer
 * @overwrite: overwrite existing config data and flush delta
 *
 * committing may reload the whole uci_package data,
 * the supplied pointer is updated accordingly
 */
extern int uci_commit(struct uci_context *ctx, struct uci_package **p, bool overwrite);
```

`overwrite` is 0:
The event sequence:
1. Chagned delta (Process)
2. Save chagned delta (Process) to tmpfs
3. Load existing package from flash
4. Load chagned delta (tmpfs) to Process
5. Write package to flash
---
if `overwrite` is 1:
1. Chagned delta (Process)
2. Load chagned delta (tmpfs) to Process
3. Write package to flash

### `uci_save()`
Save change delta from <mark>process memory</mark> to <mark>tmpfs</mark>.
``` c
/**
 * uci_save: save change delta for a package
 * @ctx: uci context
 * @p: uci_package struct
 */
extern int uci_save(struct uci_context *ctx, struct uci_package *p);

```

### `uci_set()`
Modify UCI setting. Changing configuration setting in process memory.

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
