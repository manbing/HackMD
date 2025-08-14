---
title: Raspberry Pi 4B

---

# [Pin](https://en.wikipedia.org/wiki/Lead_(electronics))
![Raspberry-Pi-4-Pinout](https://hackmd.io/_uploads/rJY5-alDeg.png)


## Protocol

[GPIO](https://en.wikipedia.org/wiki/General-purpose_input/output), [SPI](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface), [I2C](https://en.wikipedia.org/wiki/I%C2%B2C), [UART](https://en.wikipedia.org/wiki/Universal_asynchronous_receiver-transmitter), [1-Wire](https://en.wikipedia.org/wiki/1-Wire)



``` mermaid
block-beta
  columns 3
  block:group1:3
    %% colums auto (default)
    A["SPI"]
    B["I2C"]
    C["UART"]
  end
  block:group2:3
    %% colums auto (default)
    Z["GPIO"]
  end
```

## Pinout (40-pin J8 Header)
![pi4j-rpi-4b-pinout-small](https://hackmd.io/_uploads/H1i1f988ll.png)



[Pin Numbering - Raspberry Pi 4B](https://www.pi4j.com/1.4/pins/rpi-4b.html)
[Raspberry-Pi Pinout](https://pinout.xyz/)

# Porting Temperature Sensor, DS18B20
the procedure:
1. Configure GPIO PIN action as 1-Wire protocol
2. Add DS18B20 into Device Tree
3. Probe Device



## Configure GPIO PIN to Support the Protocol
Enable 1-Wire interface in `/boot/config.txt`
``` vim
# gpio pin is 4 in default.
dtoverlay=w1-gpio,gpiopin=21
```
When CPU bootup, Bootloader will read this configuration file and configure GPIO PIN 21 as 1-Wire interface.


## Add DS18B20 into Device Tree

[devicetree-specification](https://github.com/devicetree-org/devicetree-specification)
dts, dtb, dto

``` console
$ cd /proc/device-tree/
$ cd /sys/firmware/devicetree/base
$ dtc -O dtb -o p4080ds.dtb p4080ds.dts
$ dtc -I dtb -O dts p4080ds.dtb
$ dtc -I dtb -O dts -o <output_filename.dts> <input_filename.dtbo>
$ cd /sys/bus/w1/devices/28-00000xxxxxxx/
```

## Probe Device


# Reference
[Day 8：Device Tree (Part 1)](https://ithelp.ithome.com.tw/m/articles/10242811)


# [Pin](https://en.wikipedia.org/wiki/Lead_(electronics))
![Raspberry-Pi-4-Pinout](https://hackmd.io/_uploads/rJY5-alDeg.png)


## Protocol

[GPIO](https://en.wikipedia.org/wiki/General-purpose_input/output), [SPI](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface), [I2C](https://en.wikipedia.org/wiki/I%C2%B2C), [UART](https://en.wikipedia.org/wiki/Universal_asynchronous_receiver-transmitter), [1-Wire](https://en.wikipedia.org/wiki/1-Wire)



``` mermaid
block-beta
  columns 3
  block:group1:3
    %% colums auto (default)
    A["SPI"]
    B["I2C"]
    C["UART"]
  end
  block:group2:3
    %% colums auto (default)
    Z["GPIO"]
  end
```

## Pinout (40-pin J8 Header)
![pi4j-rpi-4b-pinout-small](https://hackmd.io/_uploads/H1i1f988ll.png)



[Pin Numbering - Raspberry Pi 4B](https://www.pi4j.com/1.4/pins/rpi-4b.html)
[Raspberry-Pi Pinout](https://pinout.xyz/)

# Porting Temperature Sensor, DS18B20
the procedure:
1. Configure GPIO PIN action as 1-Wire protocol
2. Add DS18B20 into Device Tree
3. Probe Device



## Configure GPIO PIN to Support the Protocol
Enable 1-Wire interface in `/boot/config.txt`
``` vim
# gpio pin is 4 in default.
dtoverlay=w1-gpio,gpiopin=21
```
When CPU bootup, Bootloader will read this configuration file and configure GPIO PIN 21 as 1-Wire interface.


## Add DS18B20 into Device Tree

[devicetree-specification](https://github.com/devicetree-org/devicetree-specification)
dts, dtb, dto

``` console
$ cd /proc/device-tree/
$ cd /sys/firmware/devicetree/base
$ dtc -O dtb -o p4080ds.dtb p4080ds.dts
$ dtc -I dtb -O dts p4080ds.dtb
$ dtc -I dtb -O dts -o <output_filename.dts> <input_filename.dtbo>
$ cd /sys/bus/w1/devices/28-00000xxxxxxx/
```

## Probe Device


# Reference
[Day 8：Device Tree (Part 1)](https://ithelp.ithome.com.tw/m/articles/10242811)


