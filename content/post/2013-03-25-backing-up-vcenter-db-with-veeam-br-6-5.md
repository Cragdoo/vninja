---
author: Christian Mohn
comments: true
date: 2013-03-25 09:03:41+00:00
layout: post
slug: backing-up-vcenter-db-with-veeam-br-6-5
title: Backing up vCenter DB with Veeam B&R 6.5
url: /virtualization/backing-up-vcenter-db-with-veeam-br-6-5/
wordpress_id: 2489
categories:
- Virtualization
tags:
- automation
- Backup
- Ops
- realworld
- vCenter
- Veeam
- VMware
- vSphere
- workaround
---

It´s a well known problem that with Veeam Backup & Recovery Replication 6.5, and earlier, backing up the SQL server that hosts the vCenter DB poses a problem. [KB1051 VSS for vCenter](http://www.veeam.com/kb1051) outlines the issue well, and provides a workaround.

If you experience this problem, you will see entries like this in your Veeam B&R backup logs:

[![Veeam vCenterDB Backup Error](/img/VeeamvCenterBackup01-1024x214.png)](/img/VeeamvCenterBackup01.png) Veeam vCenterDB Backup Error

The workaround provided by Veeam is to create host VM-Host Affinity Rules, effectively pinning a VM to a given host, and then perform the VM backup through the host rather than through the vCenter.

While this might be a way to work around the failed backup scenario, it effectively limits some of the flexibility you have in a virtualized environment. I want to have my VMs roaming around my datacenter utilizing DRS, and by pinning the VM to a given host that flexibility is reduced for the VMs in question.

I can appreciate why this issue appears, and why the workaround works, but frankly there has to be a better way of doing this. Hopefully this issue will be sorted in the next v7 version of Veeam B&R, as there certainly are ways for Veeam to work around the issue and make this a more seamless experience for the backup administrator.


### Proposal


Why not create a new option in the backup job, where you define that this particular job should run through a host, instead of the vCenter?

If selected, Veeam B&R would query the vCenter for which host the given VM resides on, then connect to the host itself and perform the backup through that?

That way we could work without having to set VM-host Affinity Rules, yet still keep vCenter out of the loop when the actual backup is performed.

Doing such a query is pretty easy,  below is an example using PowerCLI:

{{< highlight bash >}}
Get-VMHost -VM dbserv | fl -Property Name
Name : esx02
{{< /highlight >}}


**This simple query returns the host that the given VM resides on at the given time, why not do something like this inside of Veeam B&R to make sure that vCenter DB backups work, without having to resort to VM-Host Affinity Rules?**
