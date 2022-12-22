# Configuration example
###### tags: `OpenWRT`

[The UCI system](https://openwrt.org/docs/guide-user/base-system/uci)

[TR181](https://device-data-model.broadband-forum.org/index.htm#sec:interface-layers)


umount /overlay ; sleep 1; mtd -r erase rootfs_data
mount_root

uci set network.vlan_bridge=interface
uci set network.vlan_bridge.type='bridge'
uci set network.vlan_bridge.ifname=eth0_4_3999
uci set network.vlan_bridge.ip4table='policy2'


uci set network.vDevice=device
uci set network.vDevice.type='8021q'
uci set network.vDevice.ifname='eth0.4'
uci set network.vDevice.vid='3999'
uci set network.vDevice.name='eth0_4_3999



uci add network route
uci set netwokr.@route[-1].name='policy2'

uci add network rule
uci set network.@rule[-1].in="vlan_bridge"
uci set network.@rule[-1].mark="0xFF"
uci set network.@rule[-1].lookup="100"


uci add network rule
uci set network.@rule[-1].in="internet"
uci set network.@rule[-1].mark="0xFF"
uci set network.@rule[-1].lookup="100"




uci add network interface
uci set network.@interface[-1].type='bridge'
uci delete network.@interface[-1].ifname
uci add_list network.@interface[-1].ifname='eth0.3'
uci add_list network.@interface[-1].ifname='eth0.4'
uci set network.@interface[-1].name='cindy'

uci set network.vlan_bridge=interface
uci set network.vlan_bridge.type=’bridge’


network.@device[0]=device
network.@device[0].name='br-lan'
network.@device[0].type='bridge'
uci delete network.@device[0].ports
uci add_list network.@device[0].ports='eth0.1'
uci add_list network.@device[0].ports='eth0.2'
network.@device[0].igmp_snooping='1'






uci set network.lan.ipaddr='192.168.1.1'

=====================================================================================
Econet 

uci batch << EOI
set network.xpon_auth=xpon_auth
set network.xpon_auth.pon_mode='GPON'
set network.xpon_auth.auth_type_g='sn'
set network.xpon_auth.gpon_sn_auth_type='ascii'
set network.xpon_auth.sn_ascii_password='xxxx'
set network.xpon_auth.loidPasswd='1111'
set network.xpon_auth.xpon_sn_auth_type='ascii'
set network.xpon_auth.sn='ISKT123FD188'

set network.xpon_info=xpon_info
set network.xpon_info.vendor_id='ISKT'
set network.xpon_info.hw_ver='V1.0'
set network.xpon_info.equipment_id='InnboxG94'
set network.xpon_info.omcc_ver='a0'
set network.xpon_info.hw_type='94'

//vlan interface
add network wan_vlan
set network.@wan_vlan[-1]=wan_vlan
set network.@wan_vlan[-1].payload='routed'
set network.@wan_vlan[-1].vlan_id='3900'

//vlan interface
add network wan_vlan
set network.@wan_vlan[-1]=wan_vlan
set network.@wan_vlan[-1].payload='routed'
set network.@wan_vlan[-1].vlan_id='3999'
EOI
uci commit

====================================================================================


//br1
uci batch << EOF
set network.br1=interface
set network.br1.type='bridge'
set network.br1.name="br1_name"
add_list network.br1.ifname='eth0.3'
add_list network.br1.ifname='eth0.4'


add network rule
set network.@rule[-1].in="br1"
set network.@rule[-1].mark="0xFF"
set network.@rule[-1].lookup="100"


add network route
set network.@route[-1].name='policy2'

set network.br1.ip4table='policy2'

EOF


//br1 IP
//dhcp.@dhcp[0]=dhcp

uci set network.br1_lan=interface
uci set network.br1_lan.force_link='1'
uci set network.br1_lan.proto='static'
uci set network.br1_lan.ipaddr='192.168.2.1'
uci set network.br1_lan.netmask='255.255.255.0'
uci set network.br1_lan.ip6assign='60'
uci set network.br1_lan.device='br-br1'
uci set network.br1_lan.ip4table='100'

uci set dhcp.br1=dhcp
uci set dhcp.br1.interface='br1_lan'
uci set dhcp.br1.start='100'
uci set dhcp.br1.limit='150'
uci set dhcp.br1.leasetime='12h'
uci set dhcp.br1.dhcpv4='server'




//internet
uci set network.internet=interface
uci set network.internet.ifname='pon.3900'
uci set network.internet.disabled='0'
uci set network.internet.proto='dhcp'
uci set network.internet.ip4table='100'
uci set network.internet.name=internet_name


// firewall
uci add firewall zone
uci set firewall.@zone[-1].name='lan'
uci set firewall.@zone[-1].input='ACCEPT'
uci set firewall.@zone[-1].output='ACCEPT'
uci set firewall.@zone[-1].forward='ACCEPT'
uci set firewall.@zone[-1].cwmp_natiface_instance='1'
uci add_list firewall.@zone[-1].network='br1'
uci set firewall.@zone[-1].masq='1'

uci add firewall zone
uci set firewall.@zone[-1].name='wan'
uci set firewall.@zone[-1].input='DROP'
uci set firewall.@zone[-1].output='ACCEPT'
uci set firewall.@zone[-1].forward='ACCEPT'
uci set firewall.@zone[-1].cwmp_natiface_instance='1'
uci add_list firewall.@zone[-1].network='internet'
uci set firewall.@zone[-1].masq='1'

uci add firewall forwarding
uci set firewall.@forwarding[-1].src='lan'
uci set firewall.@forwarding[-1].dest'=wan'


uci add firewall rule
uci set firewall.@rule[-1].name='test'
uci set firewall.@rule[-1].target='MARK'
uci set firewall.@rule[-1].set_mark='1014'
uci set firewall.@rule[-1].src='lan'

uci add firewall rule
uci set firewall.@rule[-1].name='test'
uci set firewall.@rule[-1].target='MARK'
uci set firewall.@rule[-1].set_mark='1014'
uci set firewall.@rule[-1].src='lan'
uci set firewall.@rule[-1].dest='*'