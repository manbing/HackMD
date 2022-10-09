# TR-181 Data model
###### tags: `CWMP`


[tr-181-2-15-1-cwmp](https://cwmp-data-models.broadband-forum.org/tr-181-2-15-1-cwmp.html)

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

Device.WiFi.SSID.{i}. ***(wifi interface object)***

Device.WiFi.AccessPoint.{i}.
- SSIDReference
**value:** Device.WiFi.SSID.{i}.


## Device.IEEE1905



## Interface object
Device.PTM.Link.{i}.MACAddress
Device.WiFi.SSID.{i}.MACAddress
Device.Ethernet.Interface.{i}.MACAddress
Device.Ethernet.Link.{i}.MACAddress


## Example

> // Bridge
> Device.Bridging.Bridge.1.Enable="1"
> Device.Bridging.Bridge.1.Port.1.Enable="1"
> Device.Bridging.Bridge.1.Port.1.Name="eth0.1"
> Device.Bridging.Bridge.1.Port.1.LowerLayers="Device.Ethernet.Interface.1."
> Device.Bridging.Bridge.1.Port.1.PVID="1014"
> Device.Bridging.Bridge.1.Port.2.Enable="1"
> Device.Bridging.Bridge.1.Port.2.Name="eth0.2"
> Device.Bridging.Bridge.1.Port.2.LowerLayers="Device.Ethernet.Interface.2."
>
> // PHY interface
> Device.Ethernet.Interface.1.Enable="1"
> Device.Ethernet.Interface.1.Name="nas10"
> Device.Ethernet.Interface.1.Upstream="true"
>Device.Ethernet.Interface.1.MACAddress="aa:bb:cc:dd::ee:ff"
>
> Device.Ethernet.Link.1.Enable="1"
> Device.Ethernet.Link.1.MACAddress="aa:bb:cc:dd::ee:ff"
> Device.Ethernet.Link.1.LowerLayers="Device.Ethernet.Interface.1."
>
> // DHCP conection
> Device.IP.Interface.1.Enable="1"
> Device.IP.Interface.1.IPv4Enable="1"
> Device.IP.Interface.1.Name="Internet"
> Device.IP.Interface.1.Type="Normal"
> Device.IP.Interface.1.LowerLayers="Device.Ethernet.Interface.1."
> Device.IP.Interface.1.Router="Device.Routing.Router.1."
>
> Device.DHCPv4.Client.1.Enable="1"
> Device.DHCPv4.Client.1.Interface="Device.IP.Interface.1."
>
> // Default routing
>  Device.Routing.Router.1.IPv4Forwarding.1.DestIPAddress=""
> Device.Routing.Router.1.IPv4Forwarding.1.DestSubnetMask=""
> Device.Routing.Router.1.IPv4Forwarding.1.Interface="Device.IP.Interface.1."
> 

