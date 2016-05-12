---
title: "Virtualization, Hypervisors - introduction"
layout: post
description: ""
robots: none
---

## Virtualization 

Virtualization is a method to make the virtual environments on top of physical resources, multiplexing them with isolation that each of the guest domains is not aware of an existence of others. 

Virtualization is a part of modern operating systems. Multiple process run on top of the same OS. 
There exists virtual memory attached to each process.
Network device is accessed by sockets.
CPU is virtualized with interruptions.
Processes are not aware of each others existence. The operating system and the underlying hardware take care of virtualization.
It allows on isolation. In case when one of them crashes other should not be affected. 
Multiple application can be used in the same time, text editors, browsers, communicators. It is hard to imagine modern operating system without that feature.
Is it possible to move the application between different resources? Well, the state of application must be save but it runs in certain environment that differs. To allow that application can run inside containers as docker, but docker virtualize the operating system and runs on top of one. Going forward when the aim is to use multiple operating system there is a place for hypervisor, such Xen or KVM. 

## Hypervisors

Hypervisors are the means to virtualization, is the piece of software occurring in one of two types.

1. Type 1 - Bare-metal or native.  
Examples: Oracle OVM for SPARC, ESXi, Hyper-V KVM and traditionally Xen. 
2. Type 2 - Hosted.  
Examples: VMware Fusion, Oracle Virtual Box, Oracle VM for x86, Solaris Zones, Parallels and VMware Workstation.

The difference is the installation of the hypervisor, directly on the dedicated hardware or on top of an operating system.

![](/assets/2016/HypervisorTypes.png) 

Xen is classified as Type 1, though it is not able to run without domain 0 and the access to the Xen's control structures is possible by a console installed at domain 0 (via [Xen Tools](../XenTools/index.html)).