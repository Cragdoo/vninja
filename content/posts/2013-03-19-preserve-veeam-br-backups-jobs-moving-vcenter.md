---
author: cmohn
comments: true
date: 2013-03-19 21:13:20+00:00
layout: post
link: http://vninja.net/virtualization/preserve-veeam-br-backups-jobs-moving-vcenter/
slug: preserve-veeam-br-backups-jobs-moving-vcenter
title: Preserve your Veeam B&R Backups Jobs when Moving vCenter
wordpress_id: 2448
categories:
- Virtualization
tags:
- Ops
- vCenter
- Veeam
- Veeam Backup &amp; Replication
- VMware
- vSphere
- workaround
---

This week I´m working at a client site, upgrading their entire existing vSphere 4.1 infrastructure to vSphere 5.1. The customer engagement also includes upgrading [Veeam Backup and Replication](http://www.veeam.com/vmware-esx-backup.html) 6.0 to 6.5, and usually an isolated upgrade of Veeam B&R is a no-brainer _next_, _next_, _next_, _done_ install.

To complicate things in this particular environment, I also had to migrate the vCenter SQL DB from a local MS SQL Server 2005 Express instance to a full-fledged MS SQL Server 2008 R2 instance.

Of course, it was also decided to move the vCenter installation from a physical server, to a VM in the same operation.

To be able to have an exit path, until the vSphere 4.1 hosts management agents, were reconfigured, a new vCenter VM was created with a new IP-adress and server name.

After the migration from vCenter 4.1 to 5.1, and from physical to virtual was completed, the existing Veeam B&R server was upgraded to the latest 6.5 release.

Now, this is where things started to get a bit interesting with regards to the existing Veeam B&R backup jobs.  As far as it was concerned, the new vCenter was unknown and the old one was no longer anywhere to be found on the network.

If you add the new vCenter to the Veeam B&R server, you will need to either redefine all the existing backup jobs by adding the same VM from the new vCenter to the existing job, and keep the old one around until your retention period has passed (and have it fail on the existing VM object until it´s removed) or you will have to recreate all the jobs pointing to the new vCenter instance and start new "first time backups" for all the VMs.

For some reason you simply can not rename/redefine your vCenter connection inside of the Veeam B&R GUI.

Thankfully there is an easy way to work around this issue, without having to mess about with the Veeam B&R database: **Create a DNS alias!**

_That´s right, the solution was that simple. I created a DNS [CNAME](http://en.wikipedia.org/wiki/CNAME_record)  alias pointing the old vCenter network name to the new vCenter network name. After doing that, I had to re-enter the credentials for the vCenter connection in Veeam B&R to force a reconnect, and all of a sudden all existing backups jobs where present again and working as intended._

The reason this works, is that when you change the vCenter name and/or IP address (or even move it to a new server) it does not change the VM identification number (vmid) or Managed Object Reference (MoRef), in essence the VMs stay the same and Veeam B&R can continue managing them as before.

**Now, can we please get an option to update the registered vCenter name in Veeam Backup & Replication v7.0?**

**Update:**

Check out [Veeam vCenter Migration Utility](http://vninja.net/virtualization/veeam-vcenter-migration-utility/) for a small utility to help you move Veeam B&R jobs after replacing your vCenter.
