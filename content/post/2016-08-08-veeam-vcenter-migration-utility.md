---
author: Christian Mohn
comments: true
date: 2016-08-08 09:35:47+00:00
layout: post
slug: veeam-vcenter-migration-utility
title: Veeam vCenter Migration Utility
url: /virtualization/veeam-vcenter-migration-utility/
wordpress_id: 4138
categories:
- Virtualization
tags:
- Backup
- Backup &amp; Replication
- MoRef
- vCenter
- Veeam
---

Way back in 2013, I published [Preserve your Veeam B&R Backups Jobs when Moving vCenter](http://vninja.net/virtualization/preserve-veeam-br-backups-jobs-moving-vcenter/), outlining how to "cheat" (by using a CNAME alias) to preserve your Veeam Backup & Replication jobs if you replace your VMware vCenter.

Naturally, when there is a new vCenter instance, all the Virtual Machine Managed Object Reference's (MoRef) change, which makes Veeam Backup & Replication start a new backup/replication chain, since all VMs are treated as new ones. Not ideal by any means, but at least you wouldn't have to recreate all your jobs.

<!--more-->


**Veeam has now made a tool available that can map old MoRef's to new MoRef's in your backup jobs, in order to keep your incremental chains intact even after replacing your vCenter.**

Check out [vCenter Migration Utility](https://www.veeam.com/kb2136)!
