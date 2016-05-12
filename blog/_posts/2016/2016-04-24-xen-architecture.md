---
title: "The Xen Architecture"
layout: post
description: ""
robots: none
---

## Architecture basics

Xen is located between guests' operating systems and a hardware. It runs in the higher protection level then an operating system and user applications. In typical Intel x86 there are four protection levels - rings, numbered from 0 to 1. Operating systems which typically lies in the zero ring when run on top of Xen is moved to ring 1, Xen is placed in ring 0 and the applications run in ring 3. Because of ring 1 and 2 are rarely used some of the architectures lack them. In that case an operating system is running on the same level as applications. Modern Intel and AMD architectures support hardware virtualization adding a new protection level, [ring -1](../HWsupport/index.html), where Xen is located. 

When an operating system is shifted to the higher ring there are several changes in the way it can process. 

1. During [the boot time](../Booting/index.html) of an operating system CPU is in protected mode.
2. An operating system cannot issues [syscalls](../XenHypercall/index.html) anymore.
3. The [interrupts](../XenEvents/index.html) are driven by Xen.
4. The CPU time must be maintained, since real CPU time is shared between domains.

To run Xen the privileged domain, referred as domain 0, must exists on top of it. 

![](/assets/2016/XenArchitecture.png)

### Domain 0 
 
Domain 0, referred as a host domain, is a privileged domain, it may access the hardware directly and run the Xen control tools to manage other domains. 

### Unprivileged domain

Domain U or paravirtualized domains(PV domains), by default they do not have an access to the hardware. They need to issue the requests to hardware via the domain 0 (or a driver domain). To accomplish that communication the guest operating system must be ported to Xen architecture. The guest must implement split device drivers for network and block devices and callbacks for hypercalls. 

### HVM domain

Hardware Virtual Machine is an unmodified machine. It must run in an emulated environment, in Xen QEMU is used to for that. 
