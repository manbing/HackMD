# ubus (OpenWrt micro bus architecture)
###### tags: `OpenWRT`

# shell API
> ubus list
ubus -v list network.wireless
ubus call network reload
ubus list network.interface.*

> ubus list
ubus -v list testbed.girl
ubus call testbed.girl restart
ubus call testbed.girl reload

> ubus call network.interface.iphost_4001 status | jsonfilter -e '@["ipv4-address"][0].address'
ubus call network.interface.iphost_4001 status | jsonfilter -e '@.inactive'
ubus call network.interface.iphost_4001 status | jsonfilter -e '@["inactive"]["route"][0].nexthop'
ubus call network.device status  '{"name":"pon.4001"}'

ubus call network.interface dump | jsonfilter -e "@.interface[*]['data']['ntpserver']"
ubus call network.interface dump | jsonfilter -e "@.interface[3]['data']['ntpserver']"

# C API

Invoke / reply
>ubus_invoke()

Subscribe / notify(publisher)
>ubus_notify()
ubus_register_subscriber()
ubus_lookup_id()
ubus_subscribe()


Broadcast event

