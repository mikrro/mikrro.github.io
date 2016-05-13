---
title: "Xen Grant Tables"
layout: post
description: "Memory Sharing in Xen"
robots: none
---

The Xen architecture requires an interdomain communication to access physical devices by unprivileged domains. Moreover there may be a need to allow a communication between other domains to save memory or other resources. The communication is achieved by shared memory in Xen called **grant tables**. 

The grant table consists of grant pages. Each grant page has a constant size and posses an identifier - a grant reference. A hypercall `HYPERVISOR_grant_table_op` is an interface to manage the grant table. 

| `HYPERVISOR_grant_table_op` | `(unsigned int cmd, void *uop, unsigned int count)` |
| ------: | ----------- |
| *cmd*    | the type of operation |
| **uop*   | the operation's corresponding structure |
| *count*  | the number of operation to perform |

The main operations are:  

| description | type | struct | 
| ------ | ----------- |  ----------- |
| Acquiring an access to the grant table | `GNTTABOP_setup_table` | `gnttab_setup_table` |
| Create a page to share between domains | `GNTTABOP_map_grant_ref` |  `gnttab_map_grant_ref` |
| Move a page ownership from one domain to another | `GNTTABOP_transfer` | `gnttab_transfer` |
| Copy a data between pages owned by different domains | `GNTTABOP_copy` | `gnttab_copy` |

#### Setup table  

The operation is processed on a local copy of the `struct gnttab_setup_table`. A sanity check is performed to check if not more then max_grant frames are requested. Then a domain structure is accessed and locked. The permissions are checked and the lock on the grant table structure of the domain is acquired. Then the number of available frames is checked if it is not less then number of requested frames. /*or*/ The physical frame numbers are mapped to the guest address space. Finally, the data is written back to the `struct gnttab_setup_table`. Before the setup pages should be already allocated. 

The operation is called during starting the domain via *xl create*. It makes a call to a *libxc* library's `xc_dom_gnttab_seed` function taking care of setup the grant table for guests.

#### Grant copy

The operation passed as `struct gnttab_copy` stores the data of a source and a destination and a length of data to copy. Buffers `struct gnttab_copy_buf` are used to perform the operation. First the domains are locked and buffers are prepared. For a source domain a `__acquire_grant_for_copy` function is called and for a destination domain a `__get_paged_frame`. They fill a bufffers' page and frame, acquiring the page from domain or translating the destination page frame number to a machine frame number. Boundaries of the mapped region to copy are checked and memory is copied  

```
memcpy(dest->virt + op->dest.offset,   
	src->virt + op->source.offset, op->len);  
```  
After the success the destination page is marked as dirty. The buffers are released and domains unlocked.