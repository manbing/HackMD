# TCPDump/Wireshark
###### tags: `Networking`

## TCPDump
### Filter ICMPv6 Router Advertisement.
tcpdump -i eth0.1 -nn -e -vv "ip6[0]=134"

tcpdump -i eth0.4 -nn -e \( port 67 or port 68\)
> filter DHCP packet

tcpdump -i ra0 -nn -e \(\(ether src 80:a5:89:65:4e:a6 or ether dst 80:a5:89:65:4e:a6\) and \(port 67 or port 68\)\)


tcpdump -i eth0.4 -nn icmp
> filter ICMP packet

tcpdump -i br0 'dst or src 192.168.64.221'

tcpdump -i br0 ether host 54:c2:50:49:56:38

## Wireshark
wlan.fixed.category_code == 5 // Radio Resource Management
wlan.fixed.action_code == 4 // neighbor report request
wlan.fixed.action_code == 5 // neighbor report response

wlan.fixed.category_code == 10 // Wireless Network Management
wlan.fixed.action_code == 7
wlan.fixed.action_code == 8


eth.src[0:3]==00:06:5B