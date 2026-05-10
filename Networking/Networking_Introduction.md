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

## Network Model
[OSI 7 layers](https://en.wikipedia.org/wiki/OSI_model)
[TCP/IP 4 layers](https://en.wikipedia.org/wiki/Internet_protocol_suite)
[List of IP protocol numbers](https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers)
[List of network protocols (OSI model)](https://en.wikipedia.org/wiki/List_of_network_protocols_(OSI_model)#Layer_6_(Presentation_Layer))


## Connection V.S. Session
1. **Connection (The "Pipe")**
A connection represents a specific, active "pipe" between two devices, usually defined by the 5-tuple: source IP, source port, destination IP, destination port, and protocol (TCP/UDP).
* **Role:** Ensures data packet A reaches destination B.
* **Examples:** TCP connection (3-way handshake), UDP stream.
2. **Session (The "Conversation")**
A session provides the "glue" that binds multiple transactions together, even if the underlying connection drops.
* **Role:** Tracks user state, such as keeping a user logged in, managing an application's data exchange, or storing shopping cart items.
* **Examples:** An SSL/TLS session holding encryption keys for multiple connections, an HTTP session using cookies.

**The Relationship Between Them**
* **Multiple Connections, One Session:** Modern web browsing uses one session (defined by a cookie) that uses many connections (TCP) to load images, scripts, and pages.
* **FTP Example:** File Transfer Protocol (FTP) typically uses two connections (one for control, one for data) within a single session.
* **Firewalls:** Firewalls (like those from Checkpoint or Juniper) often track "sessions" as a "two-way data flow" or "bidirectional flow" that includes multiple underlying connection packets.

**Summary**
* **Connection:** Is the "how" (the transport).
* **Session:** Is the "what" (the application purpose).# Networking Introduction
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

## Network Model
[OSI 7 layers](https://en.wikipedia.org/wiki/OSI_model)
[TCP/IP 4 layers](https://en.wikipedia.org/wiki/Internet_protocol_suite)
[List of IP protocol numbers](https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers)
[List of network protocols (OSI model)](https://en.wikipedia.org/wiki/List_of_network_protocols_(OSI_model)#Layer_6_(Presentation_Layer))


## Connection V.S. Session
1. Connection (The "Pipe")
A connection represents a specific, active "pipe" between two devices, usually defined by the 5-tuple: source IP, source port, destination IP, destination port, and protocol (TCP/UDP).
* **Role:** Ensures data packet A reaches destination B.
* **Examples:** TCP connection (3-way handshake), UDP stream.
3. Session (The "Conversation")
A session provides the "glue" that binds multiple transactions together, even if the underlying connection drops.
* **Role:** Tracks user state, such as keeping a user logged in, managing an application's data exchange, or storing shopping cart items.
* **Examples:** An SSL/TLS session holding encryption keys for multiple connections, an HTTP session using cookies.

The Relationship Between Them
* **Multiple Connections, One Session:** Modern web browsing uses one session (defined by a cookie) that uses many connections (TCP) to load images, scripts, and pages.
* **FTP Example:** File Transfer Protocol (FTP) typically uses two connections (one for control, one for data) within a single session.
* **Firewalls:** Firewalls (like those from Checkpoint or Juniper) often track "sessions" as a "two-way data flow" or "bidirectional flow" that includes multiple underlying connection packets.