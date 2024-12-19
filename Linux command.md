---
title: Linux command
tags: [Command, Linux]

---

# Linux command
###### tags: `Command` `Linux`


update-grub


## apt
sudo apt-cache search $\lt$keyword$\gt$
sudo do-release-upgrade
sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com A1715D88E1DF1F24 40976EAF437D05B5 3B4FE6ACC0B21F32 A6616109451BBBF2
sudo apt-get update
sudo apt-get install git -y

**apt-get**
The source code which generated a specific binary package may be obtained using the apt-get source $\lt$package$\gt$ command. For example to obtain the source for the currently running kernel you can use the command:
```
apt-get source linux-image-unsigned-$(uname -r)
```

sudo add-apt-repository ppa:ubuntu-toolchain-r/test

## brctl (deprecated)
>brctl snoopdbg br3 on

## iw
>iw dev wlan0 info
iw wlan0 scan
iw wlan0 link
iw dev

## ip

ip tuntap add dev tap0 mode tap
ip address add dev tap0 192.168.2.1/24
ip link set dev tap0 up

### ip-link

ip link show wlan0
ip link set dev {interface} up
ip link set dev {interface} down
ip link add link eth0 name eth0.100 type vlan id 100
ip link set dev {ethN} address {address}
ip link set dev {ethN} vf {N} mac {address}
ip link set {ethN} vf {N} rate {speed}

ip link show {if name}
ip link set multicast on dev {if name}
ip link set <interface> promisc on
ip link set <old if name> name <new if name>

Show running interface only
> ip link show up

Show bridge
> ip link show type bridge

To create a bridge named `br0`, that have `eth0` and `eth1` as members:
> ip link add name br0 type bridge
ip link set dev br0 up
ip link set dev eth0 master br0
ip link set dev eth1 master br0

To remove an interface from the bridge:
> ip link set dev eth0 nomaster

destroy a bridge after no interface is member:
> ip link del br0

To add an interface to the bridge:
> ip link set <eth0> master <bridge>

Check member of the bridge
> ip link show master <bridge>

### ip-route
ip route flush cache
ip route add default via 10.0.1.1 dev pon.3900 table 100

> [Routing Cache](http://linux-ip.net/html/routing-cache.html)

ip route del default via 10.0.1.1 dev pon.3900
ip route show cache

### ip-address
ip addr flush dev br0
ip address add 192.168.110.139/24 dev ens33
ip -6 addr del fe80::ccae:a7bf:7bac:52ff/128 dev pppoe-internet
ip address del 192.168.2.1/24 dev tap0

### ip-mroute
ip mroute show

### ip-rule
ip rule add fwmark *FWMARK[/MASK]* [table TABLE_ID]
ip rule del fwmark 0xff iif br-br1 lookup 100
ip rule add fwmark 1014 table 100

### ip-neighbor
ip -s neighbor show
ip neigh flush all dev br-lan

### [ip-stats](https://man7.org/linux/man-pages/man8/ip-stats.8.html)

> ip stats set dev *DEV* l3_stats \{ on | off \}

## iptables (obsolete)
-j MARK --set-mark <marknumber in decimal form>
iptables -t mangle -A PREROUTING -i br-br1 -j MARK --set-mark 1014
iptables -S

## [nft](https://manpages.debian.org/testing/nftables/nft.8.en.html)
nft list tables


## df
df -h

## iwinfo
>iwinfo
iwinfo radio0 scan

## [LXD/LXC](https://linuxcontainers.org/)


```
lxc launch <image> <instance_name> -p <profile> --storage <storage_pool>
lxc launch images:ubuntu/20.04 ubuntu-limited -c limits.cpu=1 -c limits.memory=192MiB
lxc launch images:ubuntu/20.04 ubuntu-config < config.yaml

lxc config set [<remote>:][<container>] <key> <value> [flags]
lxc config set ubuntu-limited limits.memory 128MiB limits.cpu 1
lxc config show <instance_name>
lxc config get <instance_name> limits.cpu
lxc config device add ubuntu-container disk-storage-device disk source=/share/c1 path=/opt

lxc file push myfile.txt mycontainer/home/ubuntu/

lxc exec <instance_name> -- <command>
lxc exec <instance_name> -- su --login <user_name>


lxc profile set irene limits.cpu 1
lxc profile device set default root pool newpool
    
lxc storage create pool2 dir source=/data/lxd
    
lxc move <instance_name> --storage <target_pool_name>

lxc publish
    
distrobuilder pack-lxd
    
/usr/share/lxc/config/common.conf
    
lxc-console --name=lxc_container1 -t 0
lxc-execute --name=lxc_container1 -- ping 8.8.8.8
```
[https://linuxcontainers.org/lxd/docs/master/howto/storage_create_pool/#storage-create-pool](https://linuxcontainers.org/lxd/docs/master/howto/storage_create_pool/#storage-create-pool)
[https://linuxcontainers.org/lxc/manpages/man5/lxc.container.conf.5.html](https://linuxcontainers.org/lxc/manpages/man5/lxc.container.conf.5.html)
[https://linuxcontainers.org/lxd/docs/master/profiles/](https://linuxcontainers.org/lxd/docs/master/profiles/)
[https://linuxcontainers.org/lxd/getting-started-cli/](https://linuxcontainers.org/lxd/getting-started-cli/)
    
[https://linuxcontainers.org/lxd/docs/master/](https://linuxcontainers.org/lxd/docs/master/)
    
## [runc](https://github.com/opencontainers/runc)
sudo runc create -b ./ cindy
sudo runc list
runc spec
sudo runc kill cindy KILL
    
## docker
docker container create -t -i -v /home/manbing/GitHub/RaspberryPi/docker/work:/root/work ubuntu bash

docker cp /path/to/file1 DOCKER_ID:/path/to/file2
docker export ubuntu > ubuntu_export.tar
docker save ubuntu > ubuntu_save.tar
docker search IMAGE
docker pull IMAGE

### docker-image
docker image ls
docker image prune
docker save {image-name} {image-name}.tar
docker image import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]
> docker import http://downloads.openwrt.org/attitude_adjustment/12.09/x86/generic/openwrt-x86-generic-rootfs.tar.gz openwrt-x86-generic-rootfs

### docker-container
docker create [OPTIONS] IMAGE [COMMAND] [ARG..]
>  docker container create -t -i openwrt-x86-generic-rootfs sh

docker container start  [OPTIONS] CONTAINER ID [COMMAND] [ARG..]
> docker container start -i 123

docker attach `CONTAINER ID`
docker container ls -a
docker container prune
docker exec -it {container ID} bash
docker exec -ti --user $USER oss-dbg /bin/bash
docker container rm {container ID}
docker container kill {container ID}

docker run -d --name [NAME] IMAGE [COMMAND] [ARG..]
> docker-create and docker-start

docker container exec [OPTIONS] CONTAINER COMMAND [ARG...]

### docker-volume
docker volume ls
docker volume prune

### docker-network
docker network ls
docker network prune

### docker-compose


docker-compose.yml
```
version: '3'
services:
  openwrt:
    image: zzsrv/openwrt:latest
    container_name: openwrt
    restart: always
    privileged: true
    volumes:
      - ./network:/etc/config/network
    networks:
      - openwrt-wan
      - openwrt-lan
    command: /sbin/init

networks:
  openwrt-wan:
    external: true
  openwrt-lan:
    external: true
```





> docker-compose up -d && docker-compose logs -f


## samba
sudo smbpasswd -a {UserName} {password}
sudo apt install samba
service smbd restart

## bridge
bridge fdb show dev eth0.1
bridge mdb

### bridge-vlan
The `vlan` object from the bridge command will allow you to create ingress/egress filters on bridges.

To show if there is any vlan ingress/egress filters:
> bridge vlan show

To add rules to a given interface:
> bridge vlan add dev eth1 <vid, pvid, untagged, self, master>

To remove rules. Use the same parameters as vlan add at the end of the command to delete a specific rule.
> bridge vlan delete dev eth1
    
## diff
diff -u -p
colordiff

## net disk
mkdir {dir}
mount //{Ip}/{Home} {dir} -ousername={account},password={password}

## compare
cmp

## [tc-ct](https://man7.org/linux/man-pages/man8/tc-ct.8.html)


## sed
Get the vale of column 18
> cat /proc/78/stat | awk '{print $18}'

## shell
$ command > file 2>&1

Let us redirect both stderr and stdout (standard output):
> $ command &> /dev/console

## [repo](https://zh.wikipedia.org/zh-tw/Android)
repo forall -vc "git stash"

[Repo command reference](https://source.android.com/docs/setup/reference/repo?hl=zh-tw)

[repo-init](https://manpages.ubuntu.com/manpages/noble/man1/repo-init.1.html)

## [scp](https://man7.org/linux/man-pages/man1/scp.1.html)
scp <span class="green"> *source ... target* </span>

Copy file from local to remote
> scp /path/file1 myuser@192.168.0.1:/path/file2

Copy file from remote to local
> scp myuser@192.168.0.1:/path/file2 /path/file1


## else
lsblk -o NAME,FSTYPE,UUI
groups <userName-Here>
sudo -i
> sudo sh -c 'command1 && command2'
Ctrl+W is the standard "kill word" (aka werase). Ctrl+U kills the whole line (kill).
    
scp -P [port] [user@]192.168.1.10:~/kernel-new.img ./
    
find -iname "*.lua" | xargs grep -n --color=auto "luci"



<style>
.green {
  color: #2dbb29;
}
</style># Linux command
###### tags: `Command` `Linux`


update-grub


## apt
sudo apt-cache search $\lt$keyword$\gt$
sudo do-release-upgrade
sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com A1715D88E1DF1F24 40976EAF437D05B5 3B4FE6ACC0B21F32 A6616109451BBBF2
sudo apt-get update
sudo apt-get install git -y

**apt-get**
The source code which generated a specific binary package may be obtained using the apt-get source $\lt$package$\gt$ command. For example to obtain the source for the currently running kernel you can use the command:
```
apt-get source linux-image-unsigned-$(uname -r)
```

sudo add-apt-repository ppa:ubuntu-toolchain-r/test

## brctl (deprecated)
>brctl snoopdbg br3 on

## iw
>iw dev wlan0 info
iw wlan0 scan
iw wlan0 link
iw dev

## ip

ip tuntap add dev tap0 mode tap
ip address add dev tap0 192.168.2.1/24
ip link set dev tap0 up

### ip-link

ip link show wlan0
ip link set dev {interface} up
ip link set dev {interface} down
ip link add link eth0 name eth0.100 type vlan id 100
ip link set dev {ethN} address {address}
ip link set dev {ethN} vf {N} mac {address}
ip link set {ethN} vf {N} rate {speed}

ip link show {if name}
ip link set multicast on dev {if name}
ip link set <interface> promisc on
ip link set <old if name> name <new if name>

Show running interface only
> ip link show up

Show bridge
> ip link show type bridge

To create a bridge named `br0`, that have `eth0` and `eth1` as members:
> ip link add name br0 type bridge
ip link set dev br0 up
ip link set dev eth0 master br0
ip link set dev eth1 master br0

To remove an interface from the bridge:
> ip link set dev eth0 nomaster

destroy a bridge after no interface is member:
> ip link del br0

To add an interface to the bridge:
> ip link set <eth0> master <bridge>

Check member of the bridge
> ip link show master <bridge>

### ip-route
ip route flush cache
ip route add default via 10.0.1.1 dev pon.3900 table 100

> [Routing Cache](http://linux-ip.net/html/routing-cache.html)

ip route del default via 10.0.1.1 dev pon.3900
ip route show cache

### ip-address
ip addr flush dev br0
ip address add 192.168.110.139/24 dev ens33
ip -6 addr del fe80::ccae:a7bf:7bac:52ff/128 dev pppoe-internet
ip address del 192.168.2.1/24 dev tap0

### ip-mroute
ip mroute show

### ip-rule
ip rule add fwmark *FWMARK[/MASK]* [table TABLE_ID]
ip rule del fwmark 0xff iif br-br1 lookup 100
ip rule add fwmark 1014 table 100

### ip-neighbor
ip -s neighbor show
ip neigh flush all dev br-lan

### [ip-stats](https://man7.org/linux/man-pages/man8/ip-stats.8.html)

> ip stats set dev *DEV* l3_stats \{ on | off \}

## iptables (obsolete)
-j MARK --set-mark <marknumber in decimal form>
iptables -t mangle -A PREROUTING -i br-br1 -j MARK --set-mark 1014
iptables -S

## [nft](https://manpages.debian.org/testing/nftables/nft.8.en.html)
nft list tables


## df
df -h

## iwinfo
>iwinfo
iwinfo radio0 scan

## [LXD/LXC](https://linuxcontainers.org/)


```
lxc launch <image> <instance_name> -p <profile> --storage <storage_pool>
lxc launch images:ubuntu/20.04 ubuntu-limited -c limits.cpu=1 -c limits.memory=192MiB
lxc launch images:ubuntu/20.04 ubuntu-config < config.yaml

lxc config set [<remote>:][<container>] <key> <value> [flags]
lxc config set ubuntu-limited limits.memory 128MiB limits.cpu 1
lxc config show <instance_name>
lxc config get <instance_name> limits.cpu
lxc config device add ubuntu-container disk-storage-device disk source=/share/c1 path=/opt

lxc file push myfile.txt mycontainer/home/ubuntu/

lxc exec <instance_name> -- <command>
lxc exec <instance_name> -- su --login <user_name>


lxc profile set irene limits.cpu 1
lxc profile device set default root pool newpool
    
lxc storage create pool2 dir source=/data/lxd
    
lxc move <instance_name> --storage <target_pool_name>

lxc publish
    
distrobuilder pack-lxd
    
/usr/share/lxc/config/common.conf
    
lxc-console --name=lxc_container1 -t 0
lxc-execute --name=lxc_container1 -- ping 8.8.8.8
```
[https://linuxcontainers.org/lxd/docs/master/howto/storage_create_pool/#storage-create-pool](https://linuxcontainers.org/lxd/docs/master/howto/storage_create_pool/#storage-create-pool)
[https://linuxcontainers.org/lxc/manpages/man5/lxc.container.conf.5.html](https://linuxcontainers.org/lxc/manpages/man5/lxc.container.conf.5.html)
[https://linuxcontainers.org/lxd/docs/master/profiles/](https://linuxcontainers.org/lxd/docs/master/profiles/)
[https://linuxcontainers.org/lxd/getting-started-cli/](https://linuxcontainers.org/lxd/getting-started-cli/)
    
[https://linuxcontainers.org/lxd/docs/master/](https://linuxcontainers.org/lxd/docs/master/)
    
## [runc](https://github.com/opencontainers/runc)
sudo runc create -b ./ cindy
sudo runc list
runc spec
sudo runc kill cindy KILL
    
## docker
docker container create -t -i -v /home/manbing/GitHub/RaspberryPi/docker/work:/root/work ubuntu bash

docker cp /path/to/file1 DOCKER_ID:/path/to/file2
docker export ubuntu > ubuntu_export.tar
docker save ubuntu > ubuntu_save.tar
docker search IMAGE
docker pull IMAGE

### docker-image
docker image ls
docker image prune
docker save {image-name} {image-name}.tar
docker image import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]
> docker import http://downloads.openwrt.org/attitude_adjustment/12.09/x86/generic/openwrt-x86-generic-rootfs.tar.gz openwrt-x86-generic-rootfs

### docker-container
docker create [OPTIONS] IMAGE [COMMAND] [ARG..]
>  docker container create -t -i openwrt-x86-generic-rootfs sh

docker container start  [OPTIONS] CONTAINER ID [COMMAND] [ARG..]
> docker container start -i 123

docker attach `CONTAINER ID`
docker container ls -a
docker container prune
docker exec -it {container ID} bash
docker exec -ti --user $USER oss-dbg /bin/bash
docker container rm {container ID}
docker container kill {container ID}

docker run -d --name [NAME] IMAGE [COMMAND] [ARG..]
> docker-create and docker-start

docker container exec [OPTIONS] CONTAINER COMMAND [ARG...]

### docker-volume
docker volume ls
docker volume prune

### docker-network
docker network ls
docker network prune

### docker-compose


docker-compose.yml
```
version: '3'
services:
  openwrt:
    image: zzsrv/openwrt:latest
    container_name: openwrt
    restart: always
    privileged: true
    volumes:
      - ./network:/etc/config/network
    networks:
      - openwrt-wan
      - openwrt-lan
    command: /sbin/init

networks:
  openwrt-wan:
    external: true
  openwrt-lan:
    external: true
```





> docker-compose up -d && docker-compose logs -f


## samba
sudo smbpasswd -a {UserName} {password}
sudo apt install samba
service smbd restart

## bridge
bridge fdb show dev eth0.1
bridge mdb

### bridge-vlan
The `vlan` object from the bridge command will allow you to create ingress/egress filters on bridges.

To show if there is any vlan ingress/egress filters:
> bridge vlan show

To add rules to a given interface:
> bridge vlan add dev eth1 <vid, pvid, untagged, self, master>

To remove rules. Use the same parameters as vlan add at the end of the command to delete a specific rule.
> bridge vlan delete dev eth1
    
## diff
diff -u -p
colordiff

## net disk
mkdir {dir}
mount //{Ip}/{Home} {dir} -ousername={account},password={password}

## compare
cmp

## [tc-ct](https://man7.org/linux/man-pages/man8/tc-ct.8.html)


## sed
Get the vale of column 18
> cat /proc/78/stat | awk '{print $18}'

## shell
$ command > file 2>&1

Let us redirect both stderr and stdout (standard output):
> $ command &> /dev/console

## [repo](https://zh.wikipedia.org/zh-tw/Android)
repo forall -vc "git stash"

[Repo command reference](https://source.android.com/docs/setup/reference/repo?hl=zh-tw)

[repo-init](https://manpages.ubuntu.com/manpages/noble/man1/repo-init.1.html)

## [scp](https://man7.org/linux/man-pages/man1/scp.1.html)
scp <span class="green"> *source ... target* </span>

Copy file from local to remote
> scp /path/file1 myuser@192.168.0.1:/path/file2

Copy file from remote to local
> scp myuser@192.168.0.1:/path/file2 /path/file1


## else
lsblk -o NAME,FSTYPE,UUI
groups <userName-Here>
sudo -i
> sudo sh -c 'command1 && command2'
Ctrl+W is the standard "kill word" (aka werase). Ctrl+U kills the whole line (kill).
    
scp -P [port] [user@]192.168.1.10:~/kernel-new.img ./
    
find -iname "*.lua" | xargs grep -n --color=auto "luci"



<style>
.green {
  color: #2dbb29;
}
</style>