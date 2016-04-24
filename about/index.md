---
layout: page
title: About
---

## Content
I am going to include here information I gather during the work on the project:  
[QEMU xen-blkback performance analysis and improvements][0].
The project was proposed as Outreachy Program Project, but I work on it outside the program. The author of the project proposal is Roger Pau Monné <roger.pau@citrix.com>. And the following description and outcomes come from the project page.

### Description:
The current xen-blkback implementation inside of QEMU is lacking several features found in the in-kernel Linux xen-blkback implementation. The goal of the project is to analyze the bottlenecks currently found in QEMU, and provide solutions in order to reduce them. Some of them might involve:

- Use grant-copy instead of grant-map.
- Implement support for indirect descriptors.
- Implement multi-queue support.

There's a reference implementation of the last two items in the [xen-blkback code][1] in Linux, and a technical document that describes the protocol extensions in [the Xen public headers][2].

### Outcomes: 
By the end of the project, a detailed performance analysis of the block backend inside of QEMU should be provided.


[0]: http://wiki.xenproject.org/wiki/Outreach_Program_Projects#QEMU_xen-blkback_performance_analysis_and_improvements
[1]: https://blog.xenproject.org/2013/08/07/indirect-descriptors-for-xen-pv-disks/
[2]: http://xenbits.xen.org/gitweb/?p=xen.git;a=blob;f=xen/include/public/io/blkif.h