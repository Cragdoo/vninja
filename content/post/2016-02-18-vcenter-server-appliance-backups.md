---
author: Christian Mohn
comments: true
date: 2016-02-18 18:51:14+00:00
layout: post
slug: vcenter-server-appliance-backups
title: vCenter Server Appliance Backups
url: /vmware-2/vcenter-server-appliance-backups/
wordpress_id: 4041
categories:
- VMware
tags:
- Backup
- vCenter
- VCSA
- VMware
---

For some time now I've been advocating the usage of VCSA instead of the traditional Microsoft Windows based vCenter. It has feature parity with the Windows version now, it's easier to deploy, gets right-sized out of the box and eliminates the need for an external Microsoft SQL server.

One of the questions I often face when talking about the appliance,_ is how do we handle backups?_  Most customers are comfortable with backup up Windows servers and Microsoft SQL, but quite a few have reservations when it comes to the integrated vPostgres database that the VCSA employs. One common misconception is that a VCSA backup is only crash-consistent. Thankfully vPostgres takes care of this on it's own, by using what it calls [Continuous Archiving and Point-in-Time Recovery (PITR)](http://www.postgresql.org/docs/9.4/static/continuous-archiving.html).

<!--more-->

I essence, vPostgres writes everything to a log file, in case of a system crash. Since this is done continuously, every transaction that should hit the DB is recorded and can be replayed if required. From the Postgres documentation:

_"We do not need a perfectly consistent file system backup as the starting point. Any internal inconsistency in the backup will be corrected by log replay (this is not significantly different from what happens during crash recovery). So we do not need a file system snapshot capability, just tar or a similar archiving tool."_

With regards to the VCSA, this means that your image level backups  will be consistent, and there isn't really a need to dump and export the vPostgres DB and then archive that. Yet another reason to switch to the appliance today!

Myth busted!
