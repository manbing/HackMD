PDump

## Filter ICMPv6 Router Advertisement.
tcpdump -i eth0.1 -nn -e -vv "ip6[0]=134"

tcpdump -i eth0.4 -nn -e \( port 67 or port 68\)

tcpdump -i ra0 -nn -e \(\(ether src 80:a5:89:65:4e:a6 or ether dst 80:a5:89:65:4e:a6\) and \(port 67 or port 68\)\)