---
title: "Xen HVM guest"
layout: post
description: "An example for setting up and running HVM guest under Xen."
robots: none
---

## Hardware Virtual Machine guest

Hardware Virtual Machine - HVM - guest is an unmodified machine, the code of the system does not need to be changed to work under Xen. It requires a [hardware support](../HWsupport/index.md) from the machine. Because this kind of guest unlikely implements [split device drivers](../XenDeviceModel/index.md) Xen emulates the hardware by [QEMU](../Xen-QEMU/index.md). A HVM guest has an emulated BIOS and boots in the real mode. It is possible to access a Xen's hypercall page by special instructions and issue the hypercalls in the same way as a PV guests. 

### Sample configuration file 

>##### Kernel 
builder = 'hvm'  
memory  = '4096'  
vcpus   = '1'  
boot    = "c"  
bootloader = "pygrub"

>##### Devices  
disk = ['phy:/dev/vg0/suse,hda,w',  
&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;'file:/media/iso/SUSE/openSUSE-13.2-DVD-x86_64.iso,hdc:cdrom,r']  
apic = '1'  
pae = '1'  
serial = 'pty'  
usb = '1'  
usbdevice = ['host:090c:1000']  

>##### Display  
sdl = '0'  
vnc = '1'  
vnclisten = ' '  
vncpasswd = ' '  

>##### Networking  
vif = ['bridge=xenbr0']  
dhcp = 'dhcp'

Explanation of each tag can be found [here](http://xenbits.xen.org/docs/unstable/man/xl.cfg.5.html).
Following I explain some of the configuration used underneath.

1. Kernel  
When the guest is run first time it must be booted from a bootdisk. In my case it is a cd-rom with an image of openSUSE system. The boot option in this section should be accordingly set to cd-rom (boot = "d"). After completing the process of installation the system will restart and the boot option should be changed to a hard disk (boot = "c"). Remaining option is network/PXE (boot = "n").

2. Devices  
There are three devices connected to the guest. A logical volume vg0, as a hard drive accessible from the guest as /dev/hdd, a cd-rom /dev/hdc and a usbdevice. An usb bus must be enabled (usb=1) for that option. Here the device is referred by host:USBID which can be read by the lsusb tool, there are another way to refer to it as described in the configuration manual.

3. Display  
A virtual network connection is used to connect to a guest with GUI. I am using a vncviewer. Although it demands restart during changes of the guest resolution it automatically adjusts to it in contrast to a gncviewer - it is probably configurable, but I did not find a clear explanation how to achieve rescaling on the gncviewer.

4. Networking  
The main option here is the bridge name. Following is a guide how I managed to set up the network connection via wlan0 interface.  

4.1. The bridge configuration on the host  

> */etc/network/interface*   
>#Bridge setup  
>auto xenbr0  
>iface xenbr0 inet static  
>&ensp;&ensp;&ensp;&ensp;bridge_ports wlan0  
>&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;address BRIDGE_IP  
>&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;broadcast BRIDGE_BROADCAST  
>&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;netmask BRIDGE_NETMASK  
>&ensp;&ensp;&ensp;&ensp;bridge_fd 0  
>&ensp;&ensp;&ensp;&ensp;bridge_maxwait 0  
>&ensp;&ensp;&ensp;&ensp;pre-up iwconfig wlan0 essid NETWORK  
>&ensp;&ensp;&ensp;&ensp;bridge_hw BRIDGE_MAC_ADDRESS  

4.2. Enabling nat at the host  

> iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE  

4.3. Set up a guest IP as part of the bridge network.  
4.4. Add the default gateway via the bridge.  
4.5. Set the DNS accordingly to the host configuration, which can be read via nm-tool.


