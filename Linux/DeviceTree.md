# Syntax
``` 
[label:] node-name[@unit-address] {
   [properties definitions]
   [child nodes]
}
```

Unit Address
--
In a Device Tree Source (DTS) file, a unit address is a part of a node's name that specifies its address within the address space of its parent node. It follows the node name, separated by an "at" sign (@).

**Key characteristics of unit addresses**:
* Purpose: Unit addresses uniquely identify a device within its parent's address space. This is particularly important when multiple devices of the same type are present under a single parent node (e.g., multiple I2C devices on an I2C bus).
* Format: Unit addresses are typically expressed in lowercase hexadecimal digits, without leading zeros.
Relationship with reg property: Nodes with a unit address must also have a reg property, which defines the actual register or physical memory address range of the device. The unit address usually reflects the base address or an identifier within the reg property.
* Examples:
`i2c@40003000`: Represents an I2C controller whose register map base address is 0x40003000. 
`apds9960@39`: Represents an APDS9960 sensor at I2C address 0x39 on its parent I2C bus.
`cpu@0, cpu@1`: In a multi-core system, these might represent individual CPU cores, where the unit address acts as a unique ID.
`flash@8000000`: Represents a flash device starting at physical address 0x8000000. 

In essence, the unit address provides a concise way to identify and locate a specific sub-node (device) within the hierarchical structure of the Device Tree, often directly corresponding to its physical or logical address on a bus or within a memory map.


---


Q: Why CPU can not identify peripheral (by auto configuration protocols ) in runtime, needs Device Tree Source?

**Origin and Primary Use:**
- **ACPI:** Originated in the x86 PC world and is widely used in desktops, laptops, and servers. Its primary focus is on enabling power management and configuration of hardware through a standardized interface.
- **Device Tree:** Primarily used in embedded systems, especially those based on ARM architecture. Its main purpose is to provide a static, declarative description of the hardware components and their interconnections to the kernel.

**Dynamic vs. Static:**
- **ACPI:** Offers dynamic capabilities through AML, allowing the OS to interact with and manage hardware, including power states and device configuration, based on runtime conditions.
- **Device Tree:** Provides a static description of the hardware at boot time. Any changes or dynamic management typically require recompiling or reloading the Device Tree. 

In essence: ACPI is a more dynamic and interactive interface primarily focused on power management and configuration in x86 systems, while Device Tree is a static, declarative description mainly used to inform the kernel about the hardware layout in embedded ARM systems. However, ACPI adoption in ARM embedded systems is increasing, blurring these lines in some newer platforms.


[temp](https://blog.csdn.net/weixin_45264425/article/details/128259325)

---

``` console
/sys/firmware/devicetree/base/
/proc/device-tree
/sys/firmware/fdt

$ dtc -I fs -O dts /proc/device-tree -o device_tree.dts
$ dtc -I dtb -O dts /sys/firmware/fdt -o device_tree.dts
```


# Reference
[Day 8：Device Tree (Part 1)](https://ithelp.ithome.com.tw/m/articles/10242811)
[Linux and the Devicetree](https://docs.kernel.org/devicetree/usage-model.html)
[Devicetree](https://en.wikipedia.org/wiki/Devicetree)
[Device Tree（二）：基本概念](http://www.wowotech.net/device_model/dt_basic_concept.html)