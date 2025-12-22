---
title: Networking Introduction
tags: [Networking]

---

# Networking Introduction
###### tags: `Networking`


## Classless Inter-Domain Routing(CIDR)
> e.x., 128.32.96.0/20


## Message Type

| Plug| Rceptacle |
| -------- | -------- |
| Request     | Response     |
| Query    |     |
| Solicitation     | Advertisement     |


## Multicast MAC address
or layer 2 we also have a reserved prefix to use for multicast traffic. The 24-bit MAC address prefix ``01-00-5E`` is reserved for layer 2 multicast. 

## Multicast IP address
On layer 3 IANA has reserved the class D range (``224.0.0.0`` – ``239.255.255.255``) for multicast IP addresses.


## Proxy V.S. Relay
### Proxy
* User know this process exists.
* Proxy is a more advanced way of handling requests across subnets. A proxy agent is a device, such as a firewall or a VPN gateway, that acts as a server for the clients on one interface, and acts a client for real server on another interface.

### Relay
* User can not feel this process exists.
* Relay is a simple way of passing messages between different subnets. A relay agent is a device, such as a router or a switch, that listens for requests on one interface and forwards them to another interface, where a server is reachable. The server responds to the relay agent, which then relays the response back to the client. This way, the client can obtain an IP address from the server without being on the same subnet.

## Network model
[OSI 7 layers](https://en.wikipedia.org/wiki/OSI_model)
[TCP/IP 4 layers](https://en.wikipedia.org/wiki/Internet_protocol_suite)
[List of IP protocol numbers](https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers)# Networking Introduction
###### tags: `Networking`


## Classless Inter-Domain Routing(CIDR)
> e.x., 128.32.96.0/20


## Message Type

| Plug| Rceptacle |
| -------- | -------- |
| Request     | Response     |
| Query    |     |
| Solicitation     | Advertisement     |


## Multicast MAC address
or layer 2 we also have a reserved prefix to use for multicast traffic. The 24-bit MAC address prefix ``01-00-5E`` is reserved for layer 2 multicast. 

## Multicast IP address
On layer 3 IANA has reserved the class D range (``224.0.0.0`` – ``239.255.255.255``) for multicast IP addresses.


## Proxy V.S. Relay
### Proxy
* User know this process exists.
* Proxy is a more advanced way of handling requests across subnets. A proxy agent is a device, such as a firewall or a VPN gateway, that acts as a server for the clients on one interface, and acts a client for real server on another interface.

### Relay
* User can not feel this process exists.
* Relay is a simple way of passing messages between different subnets. A relay agent is a device, such as a router or a switch, that listens for requests on one interface and forwards them to another interface, where a server is reachable. The server responds to the relay agent, which then relays the response back to the client. This way, the client can obtain an IP address from the server without being on the same subnet.

## Network model
[OSI 7 layers](https://en.wikipedia.org/wiki/OSI_model)
[TCP/IP 4 layers](https://en.wikipedia.org/wiki/Internet_protocol_suite)
[List of IP protocol numbers](https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers)