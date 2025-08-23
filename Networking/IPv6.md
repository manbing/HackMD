---
title: Internet Protocol version 6 (IPv6)
tags: [Networking]

---


# Terminology / Abbreviation
SLAAC: IPv6 StateLess Address AutoConfiguration
RS: ICMPv6 Router Solicitation
RA: ICMPv6 Router Advertisement
ULA: Unique Local Address

# Addressing Methods
## unicast
Global Unicast Address, Unique Local Address
## anycast 
## multicast
Link-local Address

# Address Formats
## Global Unicast Address Format
## Unique Local Address Format
Unique local addresses are addresses analogous to IPv4 private network addresses.
## Link-local Address Format
Link-local addresses and the loopback address have link-local scope, <mark>which means they can only be used on a single directly attached network</mark>. All other addresses (including unique local addresses) have global (or universal) scope, which means they are potentially globally routable and can be used to connect to addresses with global scope anywhere, or to addresses with link-local scope on the directly attached network.

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
<mark>If the M flag is set, the O flag is redundant and can be ignored because DHCPv6 will return all available configuration information</mark>.

* O
1-bit "Other configuration" flag.  When set, it indicates that other configuration information is available via DHCPv6.  Examples of such information are DNS-related information or information on other servers within the network.


| M bit | O bit | Mode | Description |
| -------- | -------- | -------- | -------- |
| 1     | 1/0     | Stateful DHCPv6     | Get IP, Gateway, DNS and so on information from DHCP server. |
| 0     | 0     | SLAAC     | Get IP, Gateway, DNS and so on information from RA. |
| 0     | 1     | Stateless DHCPv6     | Get IP from RA; Get DNS and so on information from DHCP server. |


# Reference
[RFC 4861 - Neighbor Discovery for IP version 6 (IPv6)](https://datatracker.ietf.org/doc/html/rfc4861)

[RFC 3315 - Dynamic Host Configuration Protocol for IPv6 (DHCPv6)](https://datatracker.ietf.org/doc/html/rfc3315)

[RFC 2460 - Internet Protocol, Version 6 (IPv6) Specification](https://datatracker.ietf.org/doc/html/rfc2460)

[IPv6 address](https://en.wikipedia.org/wiki/IPv6_address)

[IPv6 Global Unicast Address Format](https://datatracker.ietf.org/doc/html/rfc3587)
# Terminology / Abbreviation
SLAAC: IPv6 StateLess Address AutoConfiguration
RS: ICMPv6 Router Solicitation
RA: ICMPv6 Router Advertisement
ULA: Unique Local Address

# Addressing Methods
## unicast
Global Unicast Address, Unique Local Address
## anycast 
## multicast
Link-local Address

# Address Formats
## Global Unicast Address Format
## Unique Local Address Format
Unique local addresses are addresses analogous to IPv4 private network addresses.
## Link-local Address Format
Link-local addresses and the loopback address have link-local scope, <mark>which means they can only be used on a single directly attached network</mark>. All other addresses (including unique local addresses) have global (or universal) scope, which means they are potentially globally routable and can be used to connect to addresses with global scope anywhere, or to addresses with link-local scope on the directly attached network.

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
<mark>If the M flag is set, the O flag is redundant and can be ignored because DHCPv6 will return all available configuration information</mark>.

* O
1-bit "Other configuration" flag.  When set, it indicates that other configuration information is available via DHCPv6.  Examples of such information are DNS-related information or information on other servers within the network.


| M bit | O bit | Mode | Description |
| -------- | -------- | -------- | -------- |
| 1     | 1/0     | Stateful DHCPv6     | Get IP, Gateway, DNS and so on information from DHCP server. |
| 0     | 0     | SLAAC     | Get IP, Gateway, DNS and so on information from RA. |
| 0     | 1     | Stateless DHCPv6     | Get IP from RA; Get DNS and so on information from DHCP server. |


# Reference
[RFC 4861 - Neighbor Discovery for IP version 6 (IPv6)](https://datatracker.ietf.org/doc/html/rfc4861)

[RFC 3315 - Dynamic Host Configuration Protocol for IPv6 (DHCPv6)](https://datatracker.ietf.org/doc/html/rfc3315)

[RFC 2460 - Internet Protocol, Version 6 (IPv6) Specification](https://datatracker.ietf.org/doc/html/rfc2460)

[IPv6 address](https://en.wikipedia.org/wiki/IPv6_address)

[IPv6 Global Unicast Address Format](https://datatracker.ietf.org/doc/html/rfc3587)