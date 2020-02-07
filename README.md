# board bring up & boardsupportpacakages
a guide for board bring up.
# bsp coming soon.

source:U-boot boot loader and getting ready to ship
Doug Abbott, in Linux for Embedded and Real-Time Applications (Fourth Edition), 2018 

There have been a number of approaches to addressing this problem. The notion of a “board support package” or BSP attempts to gather all of the hardware-dependent code in a few files in one place. It could be argued that the entire arch/ subtree of the Linux kernel source tree is a gigantic board support package.

Take a look at the arch/arm/ subtree of the kernel. In there you will find a large number of directories of the form mach-* and plat-*, presumably short for “machine” and “platform,” respectively. Most of the files in these directories provide configuration information for a specific implementation of the ARM architecture. And of course, each implementation describes its configuration differently.

Would not it be nice to have a single language that could be used to unambiguously describe the hardware of a computer system? That is the premise, and promise, of device trees.

The peripheral devices in a system can be characterized along a number of dimensions. There are, for example, character vs block devices. There are memory mapped devices, and those that connect through an external bus such as I2C or USB. Then there are platform devices and discoverable devices.

Discoverable devices are those that live on external busses, such as PCI and USB, that can tell the system what they are and how they are configured. That is, they can be “discovered” by the kernel. Having identified a device, it is a fairly simple matter to load the corresponding driver, which then interrogates the device to determine its precise configuration.

Platform devices, on the other hand, lack any mechanism to identify themselves. System on Chip (SoC) implementations, such as the Sitara, are rife with these platform devices—system clocks, interrupt controllers, GPIO, serial ports, to name a few. The device tree mechanism is particularly useful for managing platform devices.

The device tree concept evolved in the PowerPC branch of the kernel, and that is where it seems to be used the most. In fact, it is now a requirement that all PowerPC platforms pass a device tree to the kernel at boot time. The text representation of a device tree is a file with the extension .dts. These .dts files are typically found in the kernel source tree at arch/$ARCH/boot/dts.

A device tree is a hierarchical data structure that describes the collection of devices and interconnecting busses of a computer system. It is organized as nodes that begin at a root represented by “/,” just like the root file system. Every node has a name and consists of “properties” that are name-value pairs. It may also contain “child” nodes.

Listing 15.1 is a sample device tree taken from the devicetree.org website. It does nothing beyond illustrating the structure. Here we have two nodes named node1 and node2. node1 has two child nodes, and node2 has one child. Properties are represented by name=value. Values can be strings, lists of strings, one or more numeric values enclosed by square brackets, or one or more “cells” enclosed in angle brackets. The value can also be empty if the property conveys a Boolean value by its presence or absence.

source: https://www.sciencedirect.com/topics/computer-science/board-support-package

Via device tree overlays kernel modules can be loaded at boot time i.e. on RPi adding dtoverlay=lirc-rpi to /boot/config.txt loads the lirc-pi kernel module/device driver:

    A future default config.txt may contain a section like this:

     # Uncomment some or all of these to enable the optional hardware interfaces
     #dtparam=i2c_arm=on
     #dtparam=i2s=on
     #dtparam=spi=on

    If you have an overlay that defines some parameters, they can be specified either on subsequent lines like this:

     dtoverlay=lirc-rpi 
     dtparam=gpio_out_pin=16 
     dtparam=gpio_in_pin=17
     dtparam=gpio_in_pull=down

source: https://www.raspberrypi.org/documentation/configuration/device-tree.md

When building BSPs with Yocto all the hardware information that is scattered over device drivers, the kernel, the boot loader, ... is gathered, here is the developer's guide how this can be done in Yocto https://www.yoctoproject.org/docs/2.5/bsp-guide/bsp-guide.html

    [Yocto documentation]... states that the BSP “…is concerned with the hardware-specific components only. At the end-distribution point, you can ship the BSP combined with a build system and other tools. However, it is important to maintain the distinction that these are separate components that happen to be combined in certain end products.”

source : https://www.microcontrollertips.com/board-support-package/
ano:
https://github.com/imrickysu/ZYNQ-Custom-Board-Bring-Up-Guide
