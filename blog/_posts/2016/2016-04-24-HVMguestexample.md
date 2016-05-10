---
title: "Xen HVM guest"
layout: post
description: "An example for settup and running HVM guest under Xen."
robots: none
---

## Hardware Virtual Machine guest

Hardware Virtual Machine - HVM - guest is an unmodified machine, the code of the system does not need to be changed to work under Xen. It requires [hardware support](../HWsupport/index.md). Because this kind of guest unlikely implements [slit device drivers](../XenDeviceModel/index.md) Xen emulates the hardware by [QEMU](../Xen-QEMU/index.md). HVM guest has emulated BIOS and begins in real mode. It is possible to access Xen hypercall page by special instrucations and issue them in the same way as PV guests. 

## Sample configuration file 

>##### Kernel  
>builder = 'hvm'  
>memory  = '512'  
>vcpus   = '1'  
>boot    = "c"  
>bootloader = "pygrub"

>##### Devices  
>disk    = ['phy:/dev/vg0/suse,hda,w', 'file:/media/paulina/DANE/store/iso/SUSE/openSUSE-13.2-DVD-x86_64.iso,hdc:cdrom,r']  
>acpi    = '1'  
>apic    = '1'  
>pae     = '1'  
>serial  = 'pty'  
>usb     = '1'  
>usbdevice=['host:090c:1000']  

>##### Display  
>sdl       = '0'  
>vnc       = '1'  
>opengl    = '1'  
>vnclisten = ' '  
>vncpasswd = ' '  

>##### Networking  
>vif     = ['mac=00:16:3e:01:01:01,bridge=xenbr0']  
>dhcp    = 'dhcp'  

>##### Behaviour    
>on_poweroff = 'destroy'  
>on_reboot 	 = 'restart'  
>on_crash    = 'restart' 

Explanaition of each tag can be  