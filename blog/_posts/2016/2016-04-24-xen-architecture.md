---
title: "Xen architecture"
layout: post
description: ""
robots: none
---

## Architecture basics

Xen is located between guests operating systems and hardware. It runs in higher protection level then operating sytem and user applications.
In typical Intel x86 there are four protection levels - rings, numbered from 0 to 1. Operating systems which typicaly lies in the zero ring when run on top of Xen is moved to ring 1, Xen is placed in ring 0 and the applications run in ring 3. Because of ring 1 and 2 are rearely used some of the architectures lack them. In that case an operating system is running on the same level as applications. Modern Intel and AMD architectures support hardware virtualization adding new protection level, [ring -1](../HWsupport/index.html), where Xen is located. 

When an operating system is shifted to higher ring there are several changes in the way it can process. 

1. During [the boot time](../Booting/index.html) of an operating system CPU is in protected mode.
2. An operating system cannot issues [syscalls](../XenHypercall/index.html) anymore
3. The [interrupts](../XenEvents/index.html) are driven by Xen.
4. The CPU time must be maintained, since real CPU time is shared between domains.

To run Xen the priviledged domain, refered as domain 0, must exists on top of it. 

![](/assets/2016/XenArchitecture.png)

### Domain 0 
 
Domain 0, referred as a host domain, is a priviledged domain, it may access the hardware directly and run the Xen control tools to manage other domains. 


### Unpriviledged domain

### HVM domain


