---
title: Linux Command
tags: [Command, Linux]

---

# Useful command
``` console
$ btop
$ slabtop
```

# Command Usage
update-grub


## apt
``` shell
$ sudo apt-cache search $\lt$keyword$\gt$
$ sudo do-release-upgrade
$ sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com A1715D88E1DF1F24 40976EAF437D05B5 3B4FE6ACC0B21F32 A6616109451BBBF2
$ sudo apt-get update
$ sudo apt-get install git -y
$ sudo add-apt-repository ppa:ubuntu-toolchain-r/test
```
**apt-get**
The source code which generated a specific binary package may be obtained using the `apt-get source <package>` command. For example to obtain the source for the currently running kernel you can use the command:
``` shell
$ apt-get source linux-image-unsigned-$(uname -r)
```

## brctl (DEPRECATED)
``` console
$ brctl snoopdbg br3 on
```

## grep
``` console
$ grep -rin -E "include .*INNDA_CONFIG" ./test
1:include ./INNDA_CONFIG
```

## iw
``` console
$ iw dev wlan0 info
$ iw wlan0 scan
$ iw wlan0 link
$ iw dev
$ iw wlan0 station dump
$ iw dev                          # 显示无线网络设备信息
$ iw dev wlan0 info               # 显示指定无线网络设备的详细信息
$ iw dev wlan0 scan               # 扫描周围热点信息
$ iw dev wlan0 link               # 获得连接状态
$ iw dev wlan0 station dump       # 列出所有STA信息
$ iw dev wlan0 connect wifi名称   # 连接至OPEN方式的AP
$ iw dev wlan0 connect wifi名称 2432 # 有同名热点AP时指定特定频段
$ iw dev wlan0 connect wifi名称 key 0:密码 d:1:默认密码   # 连接至WEP加密方式的AP
$ iw dev wlan0 disconnect         # 断开连接
$ iw phy                          # 显示无线设备的物理特性和功能
$ iw phy phy0 info                # 显示支持的无线标准
$ iw phy phy0 wowlan show         # 查看wowlan状态
$ iw phy phy0 wowlan enable       # 使能wowlan，漫游功能需要
$ iw list
$ iw dev ath2 info
$ iwconfig wlan1 mode monitor
```

Get contory code
``` console
$ iw reg get
```

## inxi
A command line system information tool
``` shell
$ inxi -Fxz
```

## ip
``` shell
$ ip tuntap add dev tap0 mode tap
$ ip address add dev tap0 192.168.2.1/24
$ ip link set dev tap0 up
```

### ip-link
``` shell
$ ip link show wlan0
$ ip link set dev {interface} up
$ ip link set dev {interface} down
$ ip link add link eth0 name eth0.100 type vlan id 100
$ ip link set dev {ethN} address {address}
$ ip link set dev {ethN} vf {N} mac {address}
$ ip link set {ethN} vf {N} rate {speed}
$ ip link show {if name}
$ ip link set multicast on dev {if name}
$ ip link set <interface> promisc on
$ ip link set <old if name> name <new if name>
```

To check the number of received and transmitted packets for a network interface using the `ip` command in Linux, you can use the `ip -s link` command.
``` console
$ ip -s link show
```


Show running interface only
``` shell
$ ip link show up
```

Show bridge:
``` shell
$ ip link show type bridge
$ ip link show master br0
```

To create a bridge named `br0`, that have `eth0` and `eth1` as members:
``` shell
$ ip link add name br0 type bridge
$ ip link set dev br0 up
$ ip link set dev eth0 master br0
$ ip link set dev eth1 master br0
```

To remove an interface from the bridge:
``` shell
$ ip link set dev eth0 nomaster
```

destroy a bridge after no interface is member:
``` shell
$ ip link del br0
```

To add an interface to the bridge:
``` shell
$ ip link set <eth0> master <bridge>
```

Check member of the bridge
``` shell
$ ip link show master <bridge>
```

### ip-route
``` shell
$ ip route flush cache
$ ip route add default via 10.0.1.1 dev pon.3900 table 100
```

> [Routing Cache](http://linux-ip.net/html/routing-cache.html)
``` shell
$ ip route del default via 10.0.1.1 dev pon.3900
$ ip route show cache
```

### ip-address
``` shell
$ ip addr flush dev br0
$ ip address add 192.168.110.139/24 dev ens33
$ ip -6 addr del fe80::ccae:a7bf:7bac:52ff/128 dev pppoe-internet
$ ip address del 192.168.2.1/24 dev tap0
```

### ip-mroute
``` shell
$ ip mroute show
```

### ip-rule
``` shell
$ ip rule add fwmark *FWMARK[/MASK]* [table TABLE_ID]
$ ip rule del fwmark 0xff iif br-br1 lookup 100
$ ip rule add fwmark 1014 table 100
```

### ip-neighbor
```
$ ip -s neighbor show
$ ip neigh flush all dev br-lan
```

### [ip-stats](https://man7.org/linux/man-pages/man8/ip-stats.8.html)
``` shell
$ ip stats set dev *DEV* l3_stats \{ on | off \}
```

## iptables (OBSOLETE)
``` shell
$ -j MARK --set-mark <marknumber in decimal form>
$ iptables -t mangle -A PREROUTING -i br-br1 -j MARK --set-mark 1014
$ iptables -S
$ ip6tables -S iface_PPP_wan_1_input
```
[List of IP protocol numbers](https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers)

## [nft](https://manpages.debian.org/testing/nftables/nft.8.en.html)
``` shell
$ nft list tables
```


## [dd](https://man7.org/linux/man-pages/man1/dd.1.html)


## [dbclient](https://linux.die.net/man/1/dbclient)


## df
``` shell
$ df -h
```

## dtc
To get the device tree in text from the device tree blob:
``` console
$ dtc -I dtb -O dts cindy.dtb
```

Compile device trees:
``` console
$ dtc -O dtb -o cindy.dtb cindy.dts
```

## iwinfo
``` shell
$ iwinfo
$ iwinfo radio0 scan
```

## [LXD/LXC](https://linuxcontainers.org/)
``` shell
$ lxc launch <image> <instance_name> -p <profile> --storage <storage_pool>
$ lxc launch images:ubuntu/20.04 ubuntu-limited -c limits.cpu=1 -c limits.memory=192MiB
$ lxc launch images:ubuntu/20.04 ubuntu-config < config.yaml

$ lxc config set [<remote>:][<container>] <key> <value> [flags]
$ lxc config set ubuntu-limited limits.memory 128MiB limits.cpu 1
$ lxc config show <instance_name>
$ lxc config get <instance_name> limits.cpu
$ lxc config device add ubuntu-container disk-storage-device disk source=/share/c1 path=/opt

$ lxc file push myfile.txt mycontainer/home/ubuntu/

$ lxc exec <instance_name> -- <command>
$ lxc exec <instance_name> -- su --login <user_name>


$ lxc profile set irene limits.cpu 1
$ lxc profile device set default root pool newpool
    
$ lxc storage create pool2 dir source=/data/lxd
    
$ lxc move <instance_name> --storage <target_pool_name>

$ lxc publish
    
$ distrobuilder pack-lxd
    
$ /usr/share/lxc/config/common.conf
    
$ lxc-console --name=lxc_container1 -t 0
$ lxc-execute --name=lxc_container1 -- ping 8.8.8.8
```
[https://linuxcontainers.org/lxd/docs/master/howto/storage_create_pool/#storage-create-pool](https://linuxcontainers.org/lxd/docs/master/howto/storage_create_pool/#storage-create-pool)
[https://linuxcontainers.org/lxc/manpages/man5/lxc.container.conf.5.html](https://linuxcontainers.org/lxc/manpages/man5/lxc.container.conf.5.html)
[https://linuxcontainers.org/lxd/docs/master/profiles/](https://linuxcontainers.org/lxd/docs/master/profiles/)
[https://linuxcontainers.org/lxd/getting-started-cli/](https://linuxcontainers.org/lxd/getting-started-cli/)
    
[https://linuxcontainers.org/lxd/docs/master/](https://linuxcontainers.org/lxd/docs/master/)

## [mount](https://man7.org/linux/man-pages/man8/mount.8.html)
mount -t <mark>*type*</mark> <mark>*device*</mark> <mark>*dir*</mark>
umount <mark>*dir*</mark>

## mtd
``` console
mtd erase partition_name
```

## [runc](https://github.com/opencontainers/runc)
``` shell
$ sudo runc create -b ./ cindy
$ sudo runc list
$ runc spec
$ sudo runc kill cindy KILL
```
    
## docker
    
``` shell
$ docker container create -t -i -v /home/manbing/GitHub/RaspberryPi/docker/work:/root/work ubuntu bash

$ docker run --rm -v /home/manbing/Sandbox:/home/manbing/Sandbox -it ubuntu:20.04 /bin/bash

$ docker cp /path/to/file1 DOCKER_ID:/path/to/file2
$ docker export ubuntu > ubuntu_export.tar
$ docker save ubuntu > ubuntu_save.tar
$ docker search <IMAGE>
$ docker pull <IMAGE>
$ docker run -it --init --net=host ubuntu:latest bash
$ docker run -it -v /home/manbing/dockerenv:/home/ubuntu/tmp --init james:latest bash
$ docker pull ubuntu:24.04
$ docker run --rm -u manbing -v /home/manbing:/home/manbing -v /etc/passwd:/etc/passwd -it innda:all /bin/bash
```
### /etc/docker/daemon.json
[--data-root](https://docs.docker.com/engine/deprecated/#-g-and---graph-flags-on-dockerd)
    
### [docker-image](https://docs.docker.com/reference/cli/docker/image/)
``` shell
$ docker image ls
$ docker image prune -a
$ docker save {image-name} {image-name}.tar
$ docker pull ubuntu:latest
$ docker pull ubuntu:24.04
$ docker pull --all-tags ubuntu
$ docker search ubuntu -f is-official=true
$ docker rmi <repository_name>:<tag>
$ docker rmi <image_id>

## docker image import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]
$ docker import http://downloads.openwrt.org/attitude_adjustment/12.09/x86/generic/openwrt-x86-generic-rootfs.tar.gz openwrt-x86-generic-rootfs
```

Create image with `Dockerfile` file:
``` shell
$ docker build -t dockerenv . --no-cache
```

#### [docker image save](https://docs.docker.com/reference/cli/docker/image/save/)
Save one or more images to a tar archive (streamed to STDOUT by default)
    
### docker-container
docker create [OPTIONS] IMAGE [COMMAND] [ARG..]
``` shell
$ docker container create -t -i openwrt-x86-generic-rootfs sh
```

docker container start  [OPTIONS] CONTAINER ID [COMMAND] [ARG..]
``` shell
$ docker container start -i 123
```

``` shell
$ docker attach `CONTAINER ID`
```
    
list all container:
``` shell
$ docker container ls -a
```
``` shell
$ docker container prune
$ docker exec -it {container ID} bash
$ docker exec -ti --user $USER oss-dbg /bin/bash
$ docker container rm {container ID}
$ docker container kill {container ID}
```

`docker-run` is `docker-create` and `docker-start`
``` shell
$ docker run -d --name [NAME] IMAGE [COMMAND] [ARG..]
$ docker run -t -i --net=host --mount type=bind,src=./dockerenv_data,dst=/data busybox sh
```
``` shell
$ docker container exec [OPTIONS] CONTAINER COMMAND [ARG...]
```

* [docker container cp](https://docs.docker.com/reference/cli/docker/container/cp/)
Copy files/folders between a container and the local filesystem:
``` shell
$ docker cp ./some_file CONTAINER:/work
```
### docker-volume
``` shell
$ docker volume ls
$ docker volume prune
```

### docker-network
``` shell
$ docker network ls
$ docker network prune
```

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


### docker-build (DEPRECATED)
Docker Build is one of Docker Engine's most used features. Whenever you are creating an image you are using Docker Build. Build is a key part of your software development life cycle allowing you to package and bundle your code and ship it anywhere.

create image with Dockerfile
``` console
$ docker build -t <tag> .
```
### [docker-commit](https://docs.docker.com/reference/cli/docker/container/commit/)

``` console
$ docker commit <container_id_or_name> my-new-committed-image:latest 
```
### docker-export
`docker export` only exports the container's filesystem and does not include the contents of any volumes associated with the container.

``` console
$  docker export <container_id_or_name> > my_exported_container.tar
```

### docker-pull
``` shell
$ docker pull ubuntu:latest
$ docker pull ubuntu:24.04
$ docker pull --all-tags ubuntu
```

## samba
``` shell
$ sudo smbpasswd -a {UserName} {password}
$ sudo apt install samba
$ service smbd restart
```

## bridge
``` shell
$ bridge fdb show dev eth0.1
$ bridge mdb
```

### bridge-vlan
The `vlan` object from the bridge command will allow you to create ingress/egress filters on bridges.

To show if there is any vlan ingress/egress filters:
``` shell
$ bridge vlan show
```

To add rules to a given interface:
``` shell
$ bridge vlan add dev eth1 <vid, pvid, untagged, self, master>
```

To remove rules. Use the same parameters as vlan add at the end of the command to delete a specific rule.
``` shell
$ bridge vlan delete dev eth1
```
    
## diff
``` shell
$ diff -u -p
$ colordiff
```

## compare
``` shell
$ cmp
```

## [tc-ct](https://man7.org/linux/man-pages/man8/tc-ct.8.html)


## sed
Get the vale of column 18
``` shell
> cat /proc/78/stat | awk '{print $18}'
```

## shell
``` shell
$ command > file 2>&1
```

Let us redirect both stderr and stdout (standard output):
``` shell
$ command &> /dev/console
```

## [scp](https://man7.org/linux/man-pages/man1/scp.1.html)
scp <span class="green"> *source ... target* </span>

Copy file from local to remote
``` shell
$ scp /path/file1 myuser@192.168.0.1:/path/file2
```

Copy file from remote to local
``` shell
$ scp myuser@192.168.0.1:/path/file2 /path/file1
```

## [su](https://www.man7.org/linux/man-pages/man1/su.1.html)

``` shell
$ sudo -i <Command>
```

Keep user environment variable:
``` shell
$ sudo -E Command>
```

## wpa_supplicant

    
## else
``` shell
lsblk -o NAME,FSTYPE,UUI
groups <userName-Here>
sudo -i
> sudo sh -c 'command1 && command2'
Ctrl+W is the standard "kill word" (aka werase). Ctrl+U kills the whole line (kill).
    
scp -P [port] [user@]192.168.1.10:~/kernel-new.img ./
    
find -iname "*.lua" | xargs grep -n --color=auto "luci"
```

# Function
## Install transformers

[Install transformers](https://huggingface.co/transformers/v3.5.1/installation.html)

``` shell
# Create Virtual Environment
$ python3 -m venv </path/to/directory>
$ virtualenv -p </path/to/python3> <venv_name>
    
# Activate Virtual Environment
$ source </path/to/directory>/bin/activate

# Deactive
$ deactivate
```

[venv — Creation of virtual environments](https://docs.python.org/3/library/venv.html)
[使用 pip 安裝 TensorFlow](https://www.tensorflow.org/install/pip?hl=zh-tw#virtual-environment-install)

## Mount net disk
``` shell
$ mkdir {dir}
$ mount //{Ip}/{Home} {dir} -ousername={account},password={password}
```

## Search string
``` console
$ grep <string> cat /proc/slabinfo
$ grep -rin <string>
```

<style>
.green {
  color: #2dbb29;
}
</style># Useful command
``` console
$ btop
$ slabtop
```

# Command Usage
update-grub


## apt
``` shell
$ sudo apt-cache search $\lt$keyword$\gt$
$ sudo do-release-upgrade
$ sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com A1715D88E1DF1F24 40976EAF437D05B5 3B4FE6ACC0B21F32 A6616109451BBBF2
$ sudo apt-get update
$ sudo apt-get install git -y
$ sudo add-apt-repository ppa:ubuntu-toolchain-r/test
```
**apt-get**
The source code which generated a specific binary package may be obtained using the `apt-get source <package>` command. For example to obtain the source for the currently running kernel you can use the command:
``` shell
$ apt-get source linux-image-unsigned-$(uname -r)
```

## brctl (DEPRECATED)
``` console
$ brctl snoopdbg br3 on
```

## grep
``` console
$ grep -rin -E "include .*INNDA_CONFIG" ./test
1:include ./INNDA_CONFIG
```

## iw
``` console
$ iw dev wlan0 info
$ iw wlan0 scan
$ iw wlan0 link
$ iw dev
$ iw wlan0 station dump
$ iw dev                          # 显示无线网络设备信息
$ iw dev wlan0 info               # 显示指定无线网络设备的详细信息
$ iw dev wlan0 scan               # 扫描周围热点信息
$ iw dev wlan0 link               # 获得连接状态
$ iw dev wlan0 station dump       # 列出所有STA信息
$ iw dev wlan0 connect wifi名称   # 连接至OPEN方式的AP
$ iw dev wlan0 connect wifi名称 2432 # 有同名热点AP时指定特定频段
$ iw dev wlan0 connect wifi名称 key 0:密码 d:1:默认密码   # 连接至WEP加密方式的AP
$ iw dev wlan0 disconnect         # 断开连接
$ iw phy                          # 显示无线设备的物理特性和功能
$ iw phy phy0 info                # 显示支持的无线标准
$ iw phy phy0 wowlan show         # 查看wowlan状态
$ iw phy phy0 wowlan enable       # 使能wowlan，漫游功能需要
$ iw list
$ iw dev ath2 info
$ iwconfig wlan1 mode monitor
```

Get contory code
``` console
$ iw reg get
```

## inxi
A command line system information tool
``` shell
$ inxi -Fxz
```

## ip
``` shell
$ ip tuntap add dev tap0 mode tap
$ ip address add dev tap0 192.168.2.1/24
$ ip link set dev tap0 up
```

### ip-link
``` shell
$ ip link show wlan0
$ ip link set dev {interface} up
$ ip link set dev {interface} down
$ ip link add link eth0 name eth0.100 type vlan id 100
$ ip link set dev {ethN} address {address}
$ ip link set dev {ethN} vf {N} mac {address}
$ ip link set {ethN} vf {N} rate {speed}
$ ip link show {if name}
$ ip link set multicast on dev {if name}
$ ip link set <interface> promisc on
$ ip link set <old if name> name <new if name>
```

To check the number of received and transmitted packets for a network interface using the `ip` command in Linux, you can use the `ip -s link` command.
``` console
$ ip -s link show
```


Show running interface only
``` shell
$ ip link show up
```

Show bridge:
``` shell
$ ip link show type bridge
$ ip link show master br0
```

To create a bridge named `br0`, that have `eth0` and `eth1` as members:
``` shell
$ ip link add name br0 type bridge
$ ip link set dev br0 up
$ ip link set dev eth0 master br0
$ ip link set dev eth1 master br0
```

To remove an interface from the bridge:
``` shell
$ ip link set dev eth0 nomaster
```

destroy a bridge after no interface is member:
``` shell
$ ip link del br0
```

To add an interface to the bridge:
``` shell
$ ip link set <eth0> master <bridge>
```

Check member of the bridge
``` shell
$ ip link show master <bridge>
```

### ip-route
``` shell
$ ip route flush cache
$ ip route add default via 10.0.1.1 dev pon.3900 table 100
```

> [Routing Cache](http://linux-ip.net/html/routing-cache.html)
``` shell
$ ip route del default via 10.0.1.1 dev pon.3900
$ ip route show cache
```

### ip-address
``` shell
$ ip addr flush dev br0
$ ip address add 192.168.110.139/24 dev ens33
$ ip -6 addr del fe80::ccae:a7bf:7bac:52ff/128 dev pppoe-internet
$ ip address del 192.168.2.1/24 dev tap0
```

### ip-mroute
``` shell
$ ip mroute show
```

### ip-rule
``` shell
$ ip rule add fwmark *FWMARK[/MASK]* [table TABLE_ID]
$ ip rule del fwmark 0xff iif br-br1 lookup 100
$ ip rule add fwmark 1014 table 100
```

### ip-neighbor
```
$ ip -s neighbor show
$ ip neigh flush all dev br-lan
```

### [ip-stats](https://man7.org/linux/man-pages/man8/ip-stats.8.html)
``` shell
$ ip stats set dev *DEV* l3_stats \{ on | off \}
```

## iptables (OBSOLETE)
``` shell
$ -j MARK --set-mark <marknumber in decimal form>
$ iptables -t mangle -A PREROUTING -i br-br1 -j MARK --set-mark 1014
$ iptables -S
$ ip6tables -S iface_PPP_wan_1_input
```
[List of IP protocol numbers](https://en.wikipedia.org/wiki/List_of_IP_protocol_numbers)

## [nft](https://manpages.debian.org/testing/nftables/nft.8.en.html)
``` shell
$ nft list tables
```


## [dd](https://man7.org/linux/man-pages/man1/dd.1.html)


## [dbclient](https://linux.die.net/man/1/dbclient)


## df
``` shell
$ df -h
```

## dtc
To get the device tree in text from the device tree blob:
``` console
$ dtc -I dtb -O dts cindy.dtb
```

Compile device trees:
``` console
$ dtc -O dtb -o cindy.dtb cindy.dts
```

## iwinfo
``` shell
$ iwinfo
$ iwinfo radio0 scan
```

## [LXD/LXC](https://linuxcontainers.org/)
``` shell
$ lxc launch <image> <instance_name> -p <profile> --storage <storage_pool>
$ lxc launch images:ubuntu/20.04 ubuntu-limited -c limits.cpu=1 -c limits.memory=192MiB
$ lxc launch images:ubuntu/20.04 ubuntu-config < config.yaml

$ lxc config set [<remote>:][<container>] <key> <value> [flags]
$ lxc config set ubuntu-limited limits.memory 128MiB limits.cpu 1
$ lxc config show <instance_name>
$ lxc config get <instance_name> limits.cpu
$ lxc config device add ubuntu-container disk-storage-device disk source=/share/c1 path=/opt

$ lxc file push myfile.txt mycontainer/home/ubuntu/

$ lxc exec <instance_name> -- <command>
$ lxc exec <instance_name> -- su --login <user_name>


$ lxc profile set irene limits.cpu 1
$ lxc profile device set default root pool newpool
    
$ lxc storage create pool2 dir source=/data/lxd
    
$ lxc move <instance_name> --storage <target_pool_name>

$ lxc publish
    
$ distrobuilder pack-lxd
    
$ /usr/share/lxc/config/common.conf
    
$ lxc-console --name=lxc_container1 -t 0
$ lxc-execute --name=lxc_container1 -- ping 8.8.8.8
```
[https://linuxcontainers.org/lxd/docs/master/howto/storage_create_pool/#storage-create-pool](https://linuxcontainers.org/lxd/docs/master/howto/storage_create_pool/#storage-create-pool)
[https://linuxcontainers.org/lxc/manpages/man5/lxc.container.conf.5.html](https://linuxcontainers.org/lxc/manpages/man5/lxc.container.conf.5.html)
[https://linuxcontainers.org/lxd/docs/master/profiles/](https://linuxcontainers.org/lxd/docs/master/profiles/)
[https://linuxcontainers.org/lxd/getting-started-cli/](https://linuxcontainers.org/lxd/getting-started-cli/)
    
[https://linuxcontainers.org/lxd/docs/master/](https://linuxcontainers.org/lxd/docs/master/)

## [mount](https://man7.org/linux/man-pages/man8/mount.8.html)
mount -t <mark>*type*</mark> <mark>*device*</mark> <mark>*dir*</mark>
umount <mark>*dir*</mark>

## mtd
``` console
mtd erase partition_name
```

## [runc](https://github.com/opencontainers/runc)
``` shell
$ sudo runc create -b ./ cindy
$ sudo runc list
$ runc spec
$ sudo runc kill cindy KILL
```
    
## docker
    
``` shell
$ docker container create -t -i -v /home/manbing/GitHub/RaspberryPi/docker/work:/root/work ubuntu bash

$ docker run --rm -v /home/manbing/Sandbox:/home/manbing/Sandbox -it ubuntu:20.04 /bin/bash

$ docker cp /path/to/file1 DOCKER_ID:/path/to/file2
$ docker export ubuntu > ubuntu_export.tar
$ docker save ubuntu > ubuntu_save.tar
$ docker search <IMAGE>
$ docker pull <IMAGE>
$ docker run -it --init --net=host ubuntu:latest bash
$ docker run -it -v /home/manbing/dockerenv:/home/ubuntu/tmp --init james:latest bash
$ docker pull ubuntu:24.04
$ docker run --rm -u manbing -v /home/manbing:/home/manbing -v /etc/passwd:/etc/passwd -it innda:all /bin/bash
```
### /etc/docker/daemon.json
[--data-root](https://docs.docker.com/engine/deprecated/#-g-and---graph-flags-on-dockerd)
    
### [docker-image](https://docs.docker.com/reference/cli/docker/image/)
``` shell
$ docker image ls
$ docker image prune -a
$ docker save {image-name} {image-name}.tar
$ docker pull ubuntu:latest
$ docker pull ubuntu:24.04
$ docker pull --all-tags ubuntu
$ docker search ubuntu -f is-official=true
$ docker rmi <repository_name>:<tag>
$ docker rmi <image_id>

## docker image import [OPTIONS] file|URL|- [REPOSITORY[:TAG]]
$ docker import http://downloads.openwrt.org/attitude_adjustment/12.09/x86/generic/openwrt-x86-generic-rootfs.tar.gz openwrt-x86-generic-rootfs
```

Create image with `Dockerfile` file:
``` shell
$ docker build -t dockerenv . --no-cache
```

#### [docker image save](https://docs.docker.com/reference/cli/docker/image/save/)
Save one or more images to a tar archive (streamed to STDOUT by default)
    
### docker-container
docker create [OPTIONS] IMAGE [COMMAND] [ARG..]
``` shell
$ docker container create -t -i openwrt-x86-generic-rootfs sh
```

docker container start  [OPTIONS] CONTAINER ID [COMMAND] [ARG..]
``` shell
$ docker container start -i 123
```

``` shell
$ docker attach `CONTAINER ID`
```
    
list all container:
``` shell
$ docker container ls -a
```
``` shell
$ docker container prune
$ docker exec -it {container ID} bash
$ docker exec -ti --user $USER oss-dbg /bin/bash
$ docker container rm {container ID}
$ docker container kill {container ID}
```

`docker-run` is `docker-create` and `docker-start`
``` shell
$ docker run -d --name [NAME] IMAGE [COMMAND] [ARG..]
$ docker run -t -i --net=host --mount type=bind,src=./dockerenv_data,dst=/data busybox sh
```
``` shell
$ docker container exec [OPTIONS] CONTAINER COMMAND [ARG...]
```

* [docker container cp](https://docs.docker.com/reference/cli/docker/container/cp/)
Copy files/folders between a container and the local filesystem:
``` shell
$ docker cp ./some_file CONTAINER:/work
```
### docker-volume
``` shell
$ docker volume ls
$ docker volume prune
```

### docker-network
``` shell
$ docker network ls
$ docker network prune
```

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


### docker-build (DEPRECATED)
Docker Build is one of Docker Engine's most used features. Whenever you are creating an image you are using Docker Build. Build is a key part of your software development life cycle allowing you to package and bundle your code and ship it anywhere.

create image with Dockerfile
``` console
$ docker build -t <tag> .
```
### [docker-commit](https://docs.docker.com/reference/cli/docker/container/commit/)

``` console
$ docker commit <container_id_or_name> my-new-committed-image:latest 
```
### docker-export
`docker export` only exports the container's filesystem and does not include the contents of any volumes associated with the container.

``` console
$  docker export <container_id_or_name> > my_exported_container.tar
```

### docker-pull
``` shell
$ docker pull ubuntu:latest
$ docker pull ubuntu:24.04
$ docker pull --all-tags ubuntu
```

## samba
``` shell
$ sudo smbpasswd -a {UserName} {password}
$ sudo apt install samba
$ service smbd restart
```

## bridge
``` shell
$ bridge fdb show dev eth0.1
$ bridge mdb
```

### bridge-vlan
The `vlan` object from the bridge command will allow you to create ingress/egress filters on bridges.

To show if there is any vlan ingress/egress filters:
``` shell
$ bridge vlan show
```

To add rules to a given interface:
``` shell
$ bridge vlan add dev eth1 <vid, pvid, untagged, self, master>
```

To remove rules. Use the same parameters as vlan add at the end of the command to delete a specific rule.
``` shell
$ bridge vlan delete dev eth1
```
    
## diff
``` shell
$ diff -u -p
$ colordiff
```

## compare
``` shell
$ cmp
```

## [tc-ct](https://man7.org/linux/man-pages/man8/tc-ct.8.html)


## sed
Get the vale of column 18
``` shell
> cat /proc/78/stat | awk '{print $18}'
```

## shell
``` shell
$ command > file 2>&1
```

Let us redirect both stderr and stdout (standard output):
``` shell
$ command &> /dev/console
```

## [scp](https://man7.org/linux/man-pages/man1/scp.1.html)
scp <span class="green"> *source ... target* </span>

Copy file from local to remote
``` shell
$ scp /path/file1 myuser@192.168.0.1:/path/file2
```

Copy file from remote to local
``` shell
$ scp myuser@192.168.0.1:/path/file2 /path/file1
```

## [su](https://www.man7.org/linux/man-pages/man1/su.1.html)

``` shell
$ sudo -i <Command>
```

Keep user environment variable:
``` shell
$ sudo -E Command>
```

## wpa_supplicant

    
## else
``` shell
lsblk -o NAME,FSTYPE,UUI
groups <userName-Here>
sudo -i
> sudo sh -c 'command1 && command2'
Ctrl+W is the standard "kill word" (aka werase). Ctrl+U kills the whole line (kill).
    
scp -P [port] [user@]192.168.1.10:~/kernel-new.img ./
    
find -iname "*.lua" | xargs grep -n --color=auto "luci"
```

# Function
## Install transformers

[Install transformers](https://huggingface.co/transformers/v3.5.1/installation.html)

``` shell
# Create Virtual Environment
$ python3 -m venv </path/to/directory>
$ virtualenv -p </path/to/python3> <venv_name>
    
# Activate Virtual Environment
$ source </path/to/directory>/bin/activate

# Deactive
$ deactivate
```

[venv — Creation of virtual environments](https://docs.python.org/3/library/venv.html)
[使用 pip 安裝 TensorFlow](https://www.tensorflow.org/install/pip?hl=zh-tw#virtual-environment-install)

## Mount net disk
``` shell
$ mkdir {dir}
$ mount //{Ip}/{Home} {dir} -ousername={account},password={password}
```

## Search string
``` console
$ grep <string> cat /proc/slabinfo
$ grep -rin <string>
```

<style>
.green {
  color: #2dbb29;
}
</style>