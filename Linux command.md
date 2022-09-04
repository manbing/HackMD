# Linux command
###### tags: `Command` `Linux`


update-grub

## apt
sudo apt-cache search <keyword>

## brctl
>brctl snoopdbg br3 on

## iw
>iw dev wlan0 info
iw wlan0 scan
iw wlan0 link
iw dev

## ip

ip link show wlan0
ip link set dev {interface} up
ip link set dev {interface} down
ip link add link eth0 name eth0.100 type vlan id 100
ip link set dev {ethN} address {address}
ip link set dev {ethN} vf {N} mac {address}
ip link set {ethN} vf {N} rate {speed}
ip route flush cache

> [Routing Cache](http://linux-ip.net/html/routing-cache.html)

ip addr flush dev br0
ip address add 192.168.110.139/24 dev ens33

ip mroute show
ip link show {if name}
ip link set multicast on dev {if name}



## df (report free disk space)
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
docker container create -t -i -v /home/manbing/GitHub/RaspberryPi/docker/work:/work ubuntu bash

docker cp /path/to/file1 DOCKER_ID:/path/to/file2
docker export ubuntu > ubuntu_export.tar
docker save ubuntu > ubuntu_save.tar

### container
docker container start {container ID}
docker attach {container ID}
docker container ls -a
docker container prune
docker exec -it {container ID} bash
docker exec -ti --user $USER oss-dbg /bin/bash

### image
docker image ls
docker image prune
docker save {image-name} {image-name}.tar

### volume
docker volume ls
docker volume prune

### network
docker network ls
docker network prune

## samba
sudo smbpasswd -a {UserName} {password}
sudo apt install samba
service smbd restart

## bridge
bridge fdb show dev eth0.1
bridge mdb
    
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
cat /proc/78/stat | awk '{print $18}'
> get the vale of column 18

## shell
$ command > file 2>&1
    
## else
lsblk -o NAME,FSTYPE,UUI
groups <userName-Here>
sudo -i
> sudo sh -c 'command1 && command2'
Ctrl+W is the standard "kill word" (aka werase). Ctrl+U kills the whole line (kill).
    
scp -P [port] [user@]192.168.1.10:~/kernel-new.img ./
