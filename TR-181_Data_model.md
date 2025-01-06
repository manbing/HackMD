---
title: TR-181 Data model
tags: [CWMP]

---

# TR-181 Data model
###### tags: `CWMP`


[tr-181-2-15-1-cwmp](https://cwmp-data-models.broadband-forum.org/tr-181-2-15-1-cwmp.html)
[TR-181 – Device Data Model](https://device-data-model.broadband-forum.org/)
[TR-181 – Device Data Model html](https://device-data-model.broadband-forum.org/index.htm)

## Device.Bridging.
Device.Bridging.Bridge.{i}.Port.{i}.
Device.Bridging.Bridge.{i}.VLAN.{i}.
Device.Bridging.Bridge.{i}.VLANPort.{i}.

**Device.Bridging.Filter.{i}.**
- Bridge
**Value:**
Device.Bridging.Bridge.{i}.VLAN.{i}.
Device.Bridging.Bridge.{i}.


- Interface
**Value:** Device.Bridging.Bridge.{i}.Port.{i}.

## Device.IP.
- Interface.{i}.

- Interface.{i}.IPv4Address.{i}.

## Device.PPP.

## Device.WiFi.
Device.WiFi.Radio.{i}.

Device.WiFi.SSID.{i}.

Device.WiFi.AccessPoint.{i}.
- SSIDReference
**value:** Device.WiFi.SSID.{i}.


## Device.IEEE1905



## Interface object
Device.InterfaceStack.{i}.
Device.DSL.Line.{i}.
Device.DSL.Channel.{i}.
Device.DSL.BondingGroup.{i}.
Device.FAST.Line.{i}.
Device.Optical.Interface.{i}.
Device.Cellular.Interface.{i}.
Device.ATM.Link.{i}.
Device.PTM.Link.{i}.
Device.Ethernet.Interface.{i}.
Device.Ethernet.Link.{i}.
Device.Ethernet.VLANTermination.{i}.
Device.USB.Interface.{i}.
Device.MoCA.Interface.{i}.
Device.WiFi.Radio.{i}.
Device.WiFi.SSID.{i}.
Device.Bridging.Bridge.{i}.Port.{i}.
Device.PPP.Interface.{i}.
Device.GRE.Tunnel.{i}.Interface.{i}.
Device.L2TPv3.Tunnel.{i}.Interface.{i}.
Device.VXLAN.Tunnel.{i}.Interface.{i}.
Device.MAP.Domain.{i}.Interface.
Device.IEEE1905.AL.Interface.{i}.
Device.FWE.Link.{i}.


## Example
Device.InterfaceStack.{i}.

> // Bridge
> Device.Bridging.Bridge.1.Enable="1"
> Device.Bridging.Bridge.1.Port.1.Enable="1"
> Device.Bridging.Bridge.1.Port.1.LowerLayers="Device.Ethernet.Interface.1."
> Device.Bridging.Bridge.1.Port.1.PVID="1014"
> Device.Bridging.Bridge.1.Port.2.Enable="1"
> Device.Bridging.Bridge.1.Port.2.LowerLayers="Device.Ethernet.Interface.2."
> Device.Bridging.Bridge.1.Port.3.Enable="1"
> Device.Bridging.Bridge.1.Port.3.LowerLayers="Device.WiFi.SSID.1."
> Device.Bridging.Bridge.1.Port.4.Enable="1"
> Device.Bridging.Bridge.1.Port.4.LowerLayers="Device.WiFi.SSID.5."
>
> Device.Ethernet.Link.1.Enable="1"
> Device.Ethernet.Link.1.Name="Device.Bridging.Bridge.1.Port.1.,Device.Bridging.Bridge.1.Port.2.,Device.Bridging.Bridge.1.Port.3.,Device.Bridging.Bridge.1.Port.4."
>
> // PHY interface
> Device.Ethernet.Interface.1.Enable="1"
> Device.Ethernet.Interface.1.Name="nas10"
> Device.Ethernet.Interface.1.Upstream="true"
> Device.Ethernet.Interface.1.MACAddress="aa:bb:cc:dd::ee:ff"
>
> Device.WiFi.Radio.1.Enable="1"
> Device.WiFi.Radio.1.MaxSupportedSSIDs="4"
> Device.WiFi.Radio.1.SupportedFrequencyBands="2.4GHz" 
> Device.WiFi.SSID.1.MACAddress="aa:bb:cc:dd::ee:ff"
> Device.WiFi.SSID.1.LowerLayers="Device.WiFi.Radio.1."
> Device.WiFi.SSID.1.Name="ra0"
>
> Device.WiFi.Radio.2.Enable="1"
> Device.WiFi.Radio.2.MaxSupportedSSIDs="4"
> Device.WiFi.Radio.2.SupportedFrequencyBands="5GHz"
> Device.WiFi.SSID.5.MACAddress="aa:bb:cc:dd::ee:ff"
> Device.WiFi.SSID.5.LowerLayers="Device.WiFi.Radio.2."
> Device.WiFi.SSID.5.Name="rai0"
>
> // IP Interface
> Device.IP.Interface.1.Enable="1"
> Device.IP.Interface.1.IPv4Enable="1"
> Device.IP.Interface.1.Name="Internet"
> Device.IP.Interface.1.Type="Normal"
> Device.IP.Interface.1.LowerLayers="Device.Ethernet.Interface.1."
> Device.IP.Interface.1.Router="Device.Routing.Router.1."
> Device.IP.Interface.1.AutoIPEnable="1"
> 
> Device.IP.Interface.2.Enable="1"
> Device.IP.Interface.2.IPv4Enable="1"
> Device.IP.Interface.2.Name="LAN"
> Device.IP.Interface.2.Type="Normal"
> Device.IP.Interface.2.LowerLayers="Device.Ethernet.Link.1."
> Device.IP.Interface.2.Router="Device.Routing.Router.1."
> Device.IP.Interface.2.AutoIPEnable="0"
> Device.IP.Interface.2.IPv4Address.1.IPAddress="192.168.1.1"
> Device.IP.Interface.2.IPv4Address.1.AddressingType="Static"
>
> // DHCP setting
> Device.DHCPv4.Client.1.Enable="1"
> Device.DHCPv4.Client.1.Interface="Device.IP.Interface.1."
>
> // Default routing
> Device.Routing.Router.1.IPv4Forwarding.1.DestIPAddress=""
> Device.Routing.Router.1.IPv4Forwarding.1.DestSubnetMask=""
> Device.Routing.Router.1.IPv4Forwarding.1.Interface="Device.IP.Interface.1."
> 

# TR-181 Data model
###### tags: `CWMP`


[tr-181-2-15-1-cwmp](https://cwmp-data-models.broadband-forum.org/tr-181-2-15-1-cwmp.html)
[TR-181 – Device Data Model](https://device-data-model.broadband-forum.org/)
[TR-181 – Device Data Model html](https://device-data-model.broadband-forum.org/index.htm)

## Device.Bridging.
Device.Bridging.Bridge.{i}.Port.{i}.
Device.Bridging.Bridge.{i}.VLAN.{i}.
Device.Bridging.Bridge.{i}.VLANPort.{i}.

**Device.Bridging.Filter.{i}.**
- Bridge
**Value:**
Device.Bridging.Bridge.{i}.VLAN.{i}.
Device.Bridging.Bridge.{i}.


- Interface
**Value:** Device.Bridging.Bridge.{i}.Port.{i}.

## Device.IP.
- Interface.{i}.

- Interface.{i}.IPv4Address.{i}.

## Device.PPP.

## Device.WiFi.
Device.WiFi.Radio.{i}.

Device.WiFi.SSID.{i}.

Device.WiFi.AccessPoint.{i}.
- SSIDReference
**value:** Device.WiFi.SSID.{i}.


## Device.IEEE1905



## Interface object
Device.InterfaceStack.{i}.
Device.DSL.Line.{i}.
Device.DSL.Channel.{i}.
Device.DSL.BondingGroup.{i}.
Device.FAST.Line.{i}.
Device.Optical.Interface.{i}.
Device.Cellular.Interface.{i}.
Device.ATM.Link.{i}.
Device.PTM.Link.{i}.
Device.Ethernet.Interface.{i}.
Device.Ethernet.Link.{i}.
Device.Ethernet.VLANTermination.{i}.
Device.USB.Interface.{i}.
Device.MoCA.Interface.{i}.
Device.WiFi.Radio.{i}.
Device.WiFi.SSID.{i}.
Device.Bridging.Bridge.{i}.Port.{i}.
Device.PPP.Interface.{i}.
Device.GRE.Tunnel.{i}.Interface.{i}.
Device.L2TPv3.Tunnel.{i}.Interface.{i}.
Device.VXLAN.Tunnel.{i}.Interface.{i}.
Device.MAP.Domain.{i}.Interface.
Device.IEEE1905.AL.Interface.{i}.
Device.FWE.Link.{i}.


## Example
Device.InterfaceStack.{i}.

> // Bridge
> Device.Bridging.Bridge.1.Enable="1"
> Device.Bridging.Bridge.1.Port.1.Enable="1"
> Device.Bridging.Bridge.1.Port.1.LowerLayers="Device.Ethernet.Interface.1."
> Device.Bridging.Bridge.1.Port.1.PVID="1014"
> Device.Bridging.Bridge.1.Port.2.Enable="1"
> Device.Bridging.Bridge.1.Port.2.LowerLayers="Device.Ethernet.Interface.2."
> Device.Bridging.Bridge.1.Port.3.Enable="1"
> Device.Bridging.Bridge.1.Port.3.LowerLayers="Device.WiFi.SSID.1."
> Device.Bridging.Bridge.1.Port.4.Enable="1"
> Device.Bridging.Bridge.1.Port.4.LowerLayers="Device.WiFi.SSID.5."
>
> Device.Ethernet.Link.1.Enable="1"
> Device.Ethernet.Link.1.Name="Device.Bridging.Bridge.1.Port.1.,Device.Bridging.Bridge.1.Port.2.,Device.Bridging.Bridge.1.Port.3.,Device.Bridging.Bridge.1.Port.4."
>
> // PHY interface
> Device.Ethernet.Interface.1.Enable="1"
> Device.Ethernet.Interface.1.Name="nas10"
> Device.Ethernet.Interface.1.Upstream="true"
> Device.Ethernet.Interface.1.MACAddress="aa:bb:cc:dd::ee:ff"
>
> Device.WiFi.Radio.1.Enable="1"
> Device.WiFi.Radio.1.MaxSupportedSSIDs="4"
> Device.WiFi.Radio.1.SupportedFrequencyBands="2.4GHz" 
> Device.WiFi.SSID.1.MACAddress="aa:bb:cc:dd::ee:ff"
> Device.WiFi.SSID.1.LowerLayers="Device.WiFi.Radio.1."
> Device.WiFi.SSID.1.Name="ra0"
>
> Device.WiFi.Radio.2.Enable="1"
> Device.WiFi.Radio.2.MaxSupportedSSIDs="4"
> Device.WiFi.Radio.2.SupportedFrequencyBands="5GHz"
> Device.WiFi.SSID.5.MACAddress="aa:bb:cc:dd::ee:ff"
> Device.WiFi.SSID.5.LowerLayers="Device.WiFi.Radio.2."
> Device.WiFi.SSID.5.Name="rai0"
>
> // IP Interface
> Device.IP.Interface.1.Enable="1"
> Device.IP.Interface.1.IPv4Enable="1"
> Device.IP.Interface.1.Name="Internet"
> Device.IP.Interface.1.Type="Normal"
> Device.IP.Interface.1.LowerLayers="Device.Ethernet.Interface.1."
> Device.IP.Interface.1.Router="Device.Routing.Router.1."
> Device.IP.Interface.1.AutoIPEnable="1"
> 
> Device.IP.Interface.2.Enable="1"
> Device.IP.Interface.2.IPv4Enable="1"
> Device.IP.Interface.2.Name="LAN"
> Device.IP.Interface.2.Type="Normal"
> Device.IP.Interface.2.LowerLayers="Device.Ethernet.Link.1."
> Device.IP.Interface.2.Router="Device.Routing.Router.1."
> Device.IP.Interface.2.AutoIPEnable="0"
> Device.IP.Interface.2.IPv4Address.1.IPAddress="192.168.1.1"
> Device.IP.Interface.2.IPv4Address.1.AddressingType="Static"
>
> // DHCP setting
> Device.DHCPv4.Client.1.Enable="1"
> Device.DHCPv4.Client.1.Interface="Device.IP.Interface.1."
>
> // Default routing
> Device.Routing.Router.1.IPv4Forwarding.1.DestIPAddress=""
> Device.Routing.Router.1.IPv4Forwarding.1.DestSubnetMask=""
> Device.Routing.Router.1.IPv4Forwarding.1.Interface="Device.IP.Interface.1."
> 

