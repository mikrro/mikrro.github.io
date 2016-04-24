---
title: "Virtualization, Hypervisors - introduction"
layout: post
description: ""
robots: none
---

## Virtualization 

Virtualization is a technic to make the virtual environments on top of physical resources, multiplexing them with isolation that each of the guest domains is not aware of an existance of others. 

Virtualization is a part of modern operating systems. Multiple process run on top of the same OS. 
There exist virtual memory attached to each process.
Network device is accessed by sockets.
CPU is virtualized with interuptions.
Processes are not aware of each other existance. The operating system and the underlying hardware take care of virtualization.
It allows on isolation. In case when one of them crashes other should not be affected. 
Multiple application can be used in the same time, text editors, browsers, communicators. It is hard to imagine modern operating system without that feature.
Is it possible to move the application between different resources? Well, the state of application must be save but it runs in certain environment that differs. To allow that application can run inside containers as docker, but docker virtualize the operating system and runs on top of one. Going forward when the aim is to use multiple operating system there is a place for hypervisor, such Xen or KVM. 

## Hypervisors

Hypervisors are the means to virtualization, is the piece of software occuring in one of two types.

1. Type 1 - Bare-metal or native
2. Type 2 - Hosted
