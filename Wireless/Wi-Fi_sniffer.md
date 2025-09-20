---
title: Wi-Fi Sniffer
tags: [IEEE802.11]

---

# 1. Environment
Check Linux kernel version. Suggest newer than 5.15.
``` console
$ uname -r
6.14.0-29-generic
```

Install Wi-Fi driver and tools, [iwlwifi](https://wireless.docs.kernel.org/en/latest/en/users/drivers/iwlwifi.html).
`iwlwifi` is the wireless driver for **Intel’s** current wireless chips.
``` console
$ sudo apt install firmware-iwlwifi
```

``` console
$ insmod iwlwifi
```

# 2. Put your interface in monitor mode
Disable network relation process to avoid occupying Wi-Fi interface.
``` console
$ sudo systemctl stop NetworkManger
$ sudo airmon-ng check kill
```

Create monitor interface, e.g., wlp4s0f0mon, with `airmon-ng`
``` console
$ sudo apt install aircrack-ng
$ sudo airmon-ng start wlp4s0f0
```

# 3. Assign channel and bandwidth
For example, specific frequency:6.115 GHz (bnad 6, Channel 33) and bandwidth 160 MHz.
``` console
$ sudo iw dev wlp4s0f0mon set freq 6115 160MHz
```

Check whether applying is taking effect or not.
``` console
$ sudo iw dev wlp4s0f0mon info
```

Important Considerations:
1. Different county support different frequency.
2. Check what frequency does it support:
``` console
$ sudo iw phy0 info
```

# 4. Capture packets
* Tcpdump
``` console
$ sudo tcpdump -i wlp4s0f0mon -w wifi_sniffer.pcap
```
* Wireshark
``` console
$ sudo wireshark
```

[在Ubuntu上配置Intel® AX210抓包模式（监控模式）教程](https://blog.csdn.net/weixin_47877869/article/details/146399736)# 1. Environment
Check Linux kernel version. Suggest newer than 5.15.
``` console
$ uname -r
6.14.0-29-generic
```

Install Wi-Fi driver and tools, [iwlwifi](https://wireless.docs.kernel.org/en/latest/en/users/drivers/iwlwifi.html).
`iwlwifi` is the wireless driver for **Intel’s** current wireless chips.
``` console
$ sudo apt install firmware-iwlwifi
```

``` console
$ insmod iwlwifi
```

# 2. Put your interface in monitor mode
Disable network relation process to avoid occupying Wi-Fi interface.
``` console
$ sudo systemctl stop NetworkManger
$ sudo airmon-ng check kill
```

Create monitor interface, e.g., wlp4s0f0mon, with `airmon-ng`
``` console
$ sudo apt install aircrack-ng
$ sudo airmon-ng start wlp4s0f0
```

# 3. Assign channel and bandwidth
For example, specific frequency:6.115 GHz (bnad 6, Channel 33) and bandwidth 160 MHz.
``` console
$ sudo iw dev wlp4s0f0mon set freq 6115 160MHz
```

Check whether applying is taking effect or not.
``` console
$ sudo iw dev wlp4s0f0mon info
```

Important Considerations:
1. Different county support different frequency.
2. Check what frequency does it support:
``` console
$ sudo iw phy0 info
```

# 4. Capture packets
* Tcpdump
``` console
$ sudo tcpdump -i wlp4s0f0mon -w wifi_sniffer.pcap
```
* Wireshark
``` console
$ sudo wireshark
```

[在Ubuntu上配置Intel® AX210抓包模式（监控模式）教程](https://blog.csdn.net/weixin_47877869/article/details/146399736)