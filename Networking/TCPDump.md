---
title: TCPDump/Wireshark
tags: [Open Source, Networking, Command]

---

[tcpdump(1) — Linux manual page](https://man7.org/linux/man-pages/man1/tcpdump.1.html)

`-A`
> Print each packet (minus its link level header) in ASCII. Handy for capturing web pages.  No effect when -x[x] or -X[X] options are used.

`-v`, `-vv`, `-vvv`
> Increase the verbosity level of the packet output. Using more v flags provides more detailed protocol information.

# TCPDump
Filter ICMPv6 Router Advertisement.
``` console
tcpdump -i eth0.1 -nn -e -vv "ip6[0]=134"
```


filter DHCP packet
``` console
tcpdump -i eth0.4 -nn -e \( port 67 or port 68\)
```

tcpdump -i ra0 -nn -e \(\(ether src 80:a5:89:65:4e:a6 or ether dst 80:a5:89:65:4e:a6\) and \(port 67 or port 68\)\)


tcpdump -i eth0.4 -nn icmp
> filter ICMP packet

tcpdump -i br0 'dst or src 192.168.64.221'

tcpdump -i br0 ether host 54:c2:50:49:56:38

# Wireshark
wlan.fixed.category_code == 5 // Radio Resource Management
wlan.fixed.action_code == 4 // neighbor report request
wlan.fixed.action_code == 5 // neighbor report response

wlan.fixed.category_code == 10 // Wireless Network Management
wlan.fixed.action_code == 7
wlan.fixed.action_code == 8


eth.src[0:3]==00:06:5B

# 802.11 Filter

[Wireshark 802.11 Filters - Reference Sheet PDF size](https://semfionetworks.com/wp-content/uploads/2021/04/wireshark_802.11_filters_-_reference_sheet.pdf)

# VLAN[tcpdump(1) — Linux manual page](https://man7.org/linux/man-pages/man1/tcpdump.1.html)

`-A`
> Print each packet (minus its link level header) in ASCII. Handy for capturing web pages.  No effect when -x[x] or -X[X] options are used.

`-v`, `-vv`, `-vvv`
> Increase the verbosity level of the packet output. Using more v flags provides more detailed protocol information.

# TCPDump
Filter ICMPv6 Router Advertisement.
``` console
tcpdump -i eth0.1 -nn -e -vv "ip6[0]=134"
```


filter DHCP packet
``` console
tcpdump -i eth0.4 -nn -e \( port 67 or port 68\)
```

tcpdump -i ra0 -nn -e \(\(ether src 80:a5:89:65:4e:a6 or ether dst 80:a5:89:65:4e:a6\) and \(port 67 or port 68\)\)


tcpdump -i eth0.4 -nn icmp
> filter ICMP packet

tcpdump -i br0 'dst or src 192.168.64.221'

tcpdump -i br0 ether host 54:c2:50:49:56:38

# Wireshark
wlan.fixed.category_code == 5 // Radio Resource Management
wlan.fixed.action_code == 4 // neighbor report request
wlan.fixed.action_code == 5 // neighbor report response

wlan.fixed.category_code == 10 // Wireless Network Management
wlan.fixed.action_code == 7
wlan.fixed.action_code == 8


eth.src[0:3]==00:06:5B

# 802.11 Filter

[Wireshark 802.11 Filters - Reference Sheet PDF size](https://semfionetworks.com/wp-content/uploads/2021/04/wireshark_802.11_filters_-_reference_sheet.pdf)

# VLAN