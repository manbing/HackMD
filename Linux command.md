# Linux command
###### tags: `Command` `Linux`


update-grub

## iw
>iw dev wlan0 info
iw wlan0 scan
iw wlan0 link
iw dev

## ip
>ip link show wlan0
>ip addr flush dev br0
>ip address add 192.168.110.139/24 dev ens33
>ip link set dev <interface> up
>ip link set dev <interface> down

## df (report free disk space)
df -h

## iwinfo
>iwinfo
iwinfo radio0 scan

## docker
docker container create -t -i -v /home/manbing/GitHub/RaspberryPi/docker/work:/work ubuntu bash

docker cp /path/to/file1 DOCKER_ID:/path/to/file2

### container
docker container start <container ID>
docker attach <container ID>
docker container ls -a
docker container prune
docker exec -it <container ID> bash

### image
docker image ls
docker image prune
docker save <image-name> > image-name.tar

### volume
docker volume ls
docker volume prune

### network
docker network ls
docker network prune

## samba
sudo smbpasswd -a <UserName> <password>
sudo apt install samba
service smbd restart

## bridge
bridge fdb show dev eth0.1
    
## else
lsblk -o NAME,FSTYPE,UUI
groups <userName-Here>
sudo -i
> sudo sh -c 'command1 && command2'
Ctrl+W is the standard "kill word" (aka werase). Ctrl+U kills the whole line (kill).
