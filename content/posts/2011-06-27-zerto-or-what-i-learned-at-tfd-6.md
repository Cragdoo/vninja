---
author: eczerwin
comments: true
date: 2011-06-27 22:38:26+00:00
layout: post
link: http://vninja.net/virtualization/zerto-or-what-i-learned-at-tfd-6/
slug: zerto-or-what-i-learned-at-tfd-6
title: 'Zerto: Or What I Learned at Tech Field Day #6!'
wordpress_id: 1206
categories:
- Virtualization
tags:
- Disaster Recovery
- DR
- Tech Field Day
- Virtualization
- VMware
- vSphere
- Zerto
---

The last day of [Tech Field Day #6](http://techfieldday.com/2011/tfd6/) myself and all the other delegates were lucky enough to get a sneak peek at stealth startup '[Zerto](http://zerto.com)'. We weren't allowed to talk about it until the 22nd and I know I am a little slow on the punch but I currently haven't seen a lot of coverage.  Just for an initial disclosure statement my trip to Tech Field Day 6 was paid for by the vendors we visited, however, I am in no way obligated to write about them or publicize them in any manner.

Zerto is an Israeli and US based company founded by Ziv and Odem Kedem. They are doing very interesting things in the BC/DR space for the enterprise and cloud sector regarding Virtualization.  They promise host based storage agnostic replication and complete vCenter integration.  Also a nice feature VM and VMDK consistency grouping, meaning it is built for vSphere environments and replicates on a VM/VMDK level. When I did a little pressing to see how it is done it was discovered that it doesn't use vStorage APIs at all but it uses a vApp per host and a driver loaded directly into the hypervisor. That would mean it goes much deeper than Changed Block Tracking to determine incremental changes but it actually looks at the data coming thru the vSCSI stack.

It works similar to a lot of current enterprise replication products where in that it splits the IO as reads and writes are coming thru, however, instead of putting it into Array Cache it puts it into memory since it is working directly in the Hypervisor. To credit [@gabvirtualworld](http://twitter.com/@gabvirtualworld) he mentioned that it uses the VMware IOVP API to complete this task in [his post that goes a bit deeper](http://www.gabesvirtualworld.com/zerto-replication-and-disaster-recovery-the-easy-way/).

Zerto boasts application protection policies and built in support for VSS to attain better application consistency on the other side. This would be useful for example with Virtualized Exchange environments and running databases. The feature I really like is RDM replication to VMDK or the other way around.  This would be really useful if you were moving datacenters and wanted to change some things around in your storage configuration during the initial replication stage.  What I also like a lot is the ability to create checkpoints/bookmarks on your replicated VMs from different points in time just in case you had a replication of a corrupted VM or data inconsistency that you needed to go back in time to resolve (This is similar to the Recoverpoint technology).  See the video below for a quick explanation of their product:

[youtube http://www.youtube.com/watch?v=LDXZ3X9DJ-k&w;=450&h;=256]


Being kind of an old school FC Network guy and a big user of array specific replication products like SRDF and Recover point (the founders of the company actually created the Recover Point Technology and sold it to EMC) I am still very curious to see the speed and resilliency of the replication.  For instance would the built in compression and WAN optimization be enough for a massive 100TB+ environment and how would it handle the initial synchronization?

Would a product such as Riverbed Steelhead or any other WAN optimization products be able to increase the replication efficiency?  It would be very interesting over time to see what third party partnerships and certifications they develop to better the usability and maturity of their product.
