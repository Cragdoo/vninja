---
author: cmohn
comments: true
date: 2016-11-10 09:57:29+00:00
layout: post
slug: vcsa-the-default-choice-always
title: VCSA - The default choice. Always.
url: /virtualization/vcsa-the-default-choice-always/
wordpress_id: 4346
categories:
- Virtualization
tags:
- vCenter
- VCSA
- vDM30in30
- VMware
- vSphere 6.5
---

I've been a big proponent of the VMware vCenter Appliance for a long time, I even did a talk called [VCS to VCSA Converter or How a Fling Can Be Good for You!](https://www.vmug.com/p/cm/ld/fid=10931) on migrating to the VCSA at the Nordic VMUG last year.

The VCSA has gone through a few iterations and versions by now, coinciding with the vSphere releases.


<!--more-->

### History



[table id=14 /]

Since v6.0 scaling has been on par with it's Windows based counterpart, supporting the same number of  hosts and VMs.

When it comes to features, VCSA 6.5 surpasses the Windows version. New tools like the Migration Tool, vCenter High Availability, Backup / Restore and the new Management Interface are all exclusively available on the VCSA.

In my opinion, the most noteworthy of these are vCenter High Availability and the Backup / Restore options.

**vCenter High Availability** adresses one of the main concerns with vCenter in general since the discontinuation of [vCenter Server Heartbeat in June 2014](http://www.vmware.com/products/vcenter-server-heartbeat.html). This new HA setup enables you to have a passive VCSA ready if your active one should fail, with the added protection of a witness that keeps track of it all. This is a native feature of the VCSA, and not available in it's Windows counterpart (or little brother as it is now...)

The **Backup / Restore** feature is very nice as well. One of the (few) arguments I've heard surrounding running the VCSA vs the Windows vCenter is surrounding backup. Thankfully the [myth regarding image level backups](http://vninja.net/vmware-2/vcenter-server-appliance-backups/) of it was debunked in v6, but the new backup / restore functionality takes that a step further. Native backup is now available in the VCSA, either via the management interface or via a public API. The backup files (HTTP(s), FTP(s), and SCP transfer protocols are supported) make up the entire VCSA configuration, and you can restore those directly from the VCSA 6.5 ISO image in case of an emergency. This means that you can image level backups of the VCSA as usual, and script the backup file generation as added protection.

Also worth noting is that in v6.5, VMware Update Manager has been included in the VCSA, and runs natively on the appliance. The last argument for keeping your Windows vCenter has just disappeared.

**With the upcoming vSphere 6.5 release it's clear that the VCSA should be the default deployment model for a new vCenter. There is really no question about it anymore. [#migrate2vcsa](https://twitter.com/search?q=%23migrate2vcsa&src=typd) you should.**

**#vDM30in30 progress:**
[progressbar_circle percent= 26.66]
