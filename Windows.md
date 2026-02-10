# Windows command
###### tags: `Command`


Adjust specific interface:
``` console
Wireless LAN adapter Wi-Fi 2:

   Connection-specific DNS Suffix  . : lan
   Link-local IPv6 Address . . . . . : fe80::be5d:effb:7fd2:7b21%19
   IPv4 Address. . . . . . . . . . . : 192.168.11.30
   Subnet Mask . . . . . . . . . . . : 255.255.255.0
   Default Gateway . . . . . . . . . : 192.168.11.1
   
Ethernet adapter Network Main:

   Connection-specific DNS Suffix  . :
   Autoconfiguration IPv4 Address. . : 169.254.8.131
   Subnet Mask . . . . . . . . . . . : 255.255.0.0
   Default Gateway . . . . . . . . . :

$ ipconfig /release "Network Main"
$ ipconfig /renew "Network Main"
```

