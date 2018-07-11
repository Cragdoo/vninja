---
author: cmohn
comments: true
date: 2014-02-18 16:16:28+00:00
layout: post
slug: automatically-datastores-vsphere
title: Automatically Name Datastores in vSphere?
url: /vmware-2/automatically-datastores-vsphere/
wordpress_id: 3069
categories:
- VMware
tags:
- Datastore
- ESXi
- Ideas
- Policy Based Computing
- storage
- vCenter
- Virtualization
- VMware
- VSAN
- vSphere
---

[William Lam](http://twitter.com/lamw) posted "[Why you should rename the default VSAN Datastore name](http://www.virtuallyghetto.com/2014/02/why-you-should-rename-default-vsan.html)" where he outlines why the default name for VSAN data stores should be changed. Of course, I completely agree with his views on this; _Leaving it at the default might cause confusion down the line._


<!--more-->

At the end of the post, William asks the following:



<blockquote>I wonder if it would be a useful to have a feature in VSAN to automatically append the vSphere Cluster name to the default VSAN Datastore name? What do you think?</blockquote>

**The answer to that is quite simple too; Yes. It would be great to be able to append the cluster name automatically.**

But this got me thinking, wouldn't it be even better would be to use the same kind of [naming pattern scheme](http://pubs.vmware.com/view-50/index.jsp?topic=/com.vmware.view.administration.doc/GUID-26AD6C7D-553A-46CB-B8B3-DA3F6958CD9C.html) we get when provisioning Horizon View desktops, when we provision datastores? In fact, this should also be an option for other datastores, not just when using VSAN.

Imagine the possibilities if you could define datastore naming schemes in your vCenter, and add a few variables like this, for instance: _{datastoretype}-{datacentername}-{clustername/hostname}-{fixed:03}._

Then you could get automatic, and perhaps even sensible, datastore naming like this:

**local-hqdc-esxi001-001**

**iscsi-hqdc-cluster01-001**

**nfs-hqdc-cluster01-001**

**fc-hqdc-cluster01-001**

**vsan-hqdc-cluster01-001**

And so on... 

I'm sure there are other potentially even more useful variables that could be used here, perhaps even incorporating something about tiering and SLA´s (platinum/gold/silver etc.) but that would require that you knew the storage characteristics and how it maps to your naming scheme when it gets defined. But yes, we do need to be able to automatically name our datastores in a coherent matter, regardless of storage type.  

**After all, we're moving to a model of policy based computing, shouldn't naming of objects like datastores, also be ruled by policy, defined at a Datacenter level in vCenter?**
(wait a minute, why don't do the same for hosts joined to a datacenter or cluster?)
