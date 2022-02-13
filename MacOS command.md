# MacOS command
###### tags: `Command`

ssh <username>@192.168.0.1
ssh -Y <username>@192.168.1.10 -p <port>
sudo screen /dev/cu.usbserial <baud rate>
diskutil list
diskutil unmountdisk /dev/disk2
sudo dd if=Downloads/openwrt-bcm27xx-bcm2711-rpi-4-squashfs-factory.img of=/dev/disk2
diskutil eject /dev/disk2
gunzip file.txt.gz