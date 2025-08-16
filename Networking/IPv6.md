---
title: Internet Protocol version 6 (IPv6)
tags: [Networking]

---


# Terminology / Abbreviation
SLAAC: IPv6 StateLess Address AutoConfiguration
RS: ICMPv6 Router Solicitation
RA: ICMPv6 Router Advertisement

# Packet Flow
``` mermaid
sequenceDiagram
  ISP-->User: 
  User->>ISP: ICMPv6 Router Solicitation
  ISP->>User: ICMPv6 Router Advertisement
```

# ICMPv6

[Router Advertisement Message Format](https://datatracker.ietf.org/doc/html/rfc4861#section-4.2)
--

ICMP Fields:
* M
1-bit "Managed address configuration" flag. When set, it indicates that addresses are available via Dynamic Host Configuration Protocol [DHCPv6]. 

* O
1-bit "Other configuration" flag.  When set, it indicates that other configuration information is available via DHCPv6.  Examples of such information are DNS-related information or information on other servers within the network.


| M bit | O bit | Mode |
| -------- | -------- | -------- |
| 0     | 0     | SLAAC     |
| 1     | 0     |      |
| 0     | 1     | SLAAC + DHCPv6 stateless     |
| 1     | 1     | DHCPv6 statefull     |


# Reference
[RFC 4861 - Neighbor Discovery for IP version 6 (IPv6)](https://datatracker.ietf.org/doc/html/rfc4861)

[RFC 3315 - Dynamic Host Configuration Protocol for IPv6 (DHCPv6)](https://datatracker.ietf.org/doc/html/rfc3315)

[RFC 2460 - Internet Protocol, Version 6 (IPv6) Specification](https://datatracker.ietf.org/doc/html/rfc2460)

# Terminology / Abbreviation
SLAAC: IPv6 StateLess Address AutoConfiguration
RS: ICMPv6 Router Solicitation
RA: ICMPv6 Router Advertisement

# Packet Flow
``` mermaid
sequenceDiagram
  ISP-->User: 
  User->>ISP: ICMPv6 Router Solicitation
  ISP->>User: ICMPv6 Router Advertisement
```

# ICMPv6

[Router Advertisement Message Format](https://datatracker.ietf.org/doc/html/rfc4861#section-4.2)
--

ICMP Fields:
* M
1-bit "Managed address configuration" flag. When set, it indicates that addresses are available via Dynamic Host Configuration Protocol [DHCPv6]. 

* O
1-bit "Other configuration" flag.  When set, it indicates that other configuration information is available via DHCPv6.  Examples of such information are DNS-related information or information on other servers within the network.


| M bit | O bit | Mode |
| -------- | -------- | -------- |
| 0     | 0     | SLAAC     |
| 1     | 0     |      |
| 0     | 1     | SLAAC + DHCPv6 stateless     |
| 1     | 1     | DHCPv6 statefull     |


# Reference
[RFC 4861 - Neighbor Discovery for IP version 6 (IPv6)](https://datatracker.ietf.org/doc/html/rfc4861)

[RFC 3315 - Dynamic Host Configuration Protocol for IPv6 (DHCPv6)](https://datatracker.ietf.org/doc/html/rfc3315)

[RFC 2460 - Internet Protocol, Version 6 (IPv6) Specification](https://datatracker.ietf.org/doc/html/rfc2460)
