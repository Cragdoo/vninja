---
author: cmohn
comments: true
date: 2013-01-10 13:47:50+00:00
layout: post
slug: fun-games-vdr-snapshots
title: Fun and Games with VDR and Snapshots
url: /virtualization/fun-games-vdr-snapshots/
wordpress_id: 2323
categories:
- Virtualization
tags:
- Datastore
- Snapshot
- VDR
- VMware
- vSphere
---

One of the smaller improvements in vSphere 5 was the introduction of the **"Virtual machine disks consolidation is needed"** configuration alert if vSphere determines that there are orphaned snapshots for a given VM.

[![Consolidation Needed Warning](http://vninja.net/wordpress/wp-content/uploads/2013/01/Consolidation.png)](http://vninja.net/wordpress/wp-content/uploads/2013/01/Consolidation.png)

Previous versions does not show this warning message, and datastore usage could potentially skyrocket for no apparent reason if something continues to create snapshots that are not properly cleaned up when they are no longer in use. Unless there is active space monitoring for your datastores, _and there should be_, it could go on unnoticed for some time.

Running a snapshot consolidation attempts to clean up this situation, and remove the orphaned snapshots reclaiming the space they occupy.

**This video from VMware shows how this is done, and how it should work:**


For more info, have a look at [KB2003638 Consolidating snapshots in vSphere 5.x](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2003638)

While it is great that you get alerted when this happens, and that there is an option to clean it up directly in the vSphere Client, what do you do if consolidation doesn´t work for some reason?

I recently visited a client who had problems with their [VDR appliance](http://pubs.vmware.com/vsphere-50/index.jsp?topic=%2Fcom.vmware.datarecovery.admin.doc_20%2FGUID-94DB0284-9D58-4FDF-8A88-847E59982AAE.html), and every attempted backup left orphaned snapshots behind. By default VDR has a retry interval of 30 minutes for failed backups (_BackupRetryInterval=30 in datarecovery.ini_), before it times out. In the space of 30 minutes, VDR did 30 backup attempts, effectively creating 30 orphaned snapshots each time a backup was attempted. One of the affected VMs had over 300 delta files accumulated over a fairly small timeframe.

There clearly were a lot of snapshots in the datastore, but for some reason the vSphere Client Snapshot Manager did not show any snapshots for the VM. Clearly there was an inconsistency here, and after investigating the _VM.vmsd_ file it became fairly apparent what was going on. The _VM.vmsd_ file is responsible for keeping tabs of the snapshot delta-vmdk files and is the source that the Snapshot Manager uses to display and manage them.

The case here was that when a snapshot is removed, the snapshot entity in the _VM.vmsd_ is removed before the changes are made to the child disks. When VDR subsequently had problems removing the child disks, they were being left behind.

Combine this situation with the fact that the _maximum redo logs supported is 255_, you can quickly run into a situation where there are snapshots left behind that you can´t get easily rid of with the consolidate command.

Cloning the VM was not really an option, since a clone operation actually consolidates snapshots as part of the cloning process, it would also fail with the same error.

_In the end, the solution was to fire up VMware vCenter Converter, and use that to perform a V2V "conversion" of the VMs. Why does this work, when a "native" vSphere clone operation does not? The answer is surprisingly simple, vCenter Converter does not know the virtual disk structure at all. It sees only that what the operating system sees, and the OS inside the VM has no concept or knowledge of the snapshots created in vCenter._

While this fixed the immediate issue of getting rid of the orphaned snapshots and reclaim the space wasted by them, it does not fix the underlying root issue that causes VDR not to clean up after itself.

For some reason it looks like VDR, after mounting the snapshot files to the appliance, does not release them, thus retaining a lock on the snapshot files, This in turn means that the _VM.vmsd_ file is cleared for VDR snapshots, but the files are still present on on the datastore.
