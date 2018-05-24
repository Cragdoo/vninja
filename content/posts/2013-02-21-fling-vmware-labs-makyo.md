---
author: cmohn
comments: true
date: 2013-02-21 08:45:25+00:00
layout: post
link: http://vninja.net/vmware-2/fling-vmware-labs-makyo/
slug: fling-vmware-labs-makyo
title: 'New Fling from VMware Labs: Makyo'
wordpress_id: 2391
categories:
- VMware
tags:
- Fling
- Makyo
- Plugin
- Suggestion
- VMware
---

VMware Labs has released a new fling called Makyo ([魔境 _makyō_ means “ghost cave” or “devil’s cave.”](http://en.wikipedia.org/wiki/Makyo)) which basically is a tool to copy VMs or vApps from one vCenter instance to another.

It works by doing an automated OVF export on the source vCenter, and then an import on the destination vCenter.

[![mayko-summary](http://vninja.net/wordpress/wp-content/uploads/2013/02/mayko-summary-300x203.png)](http://vninja.net/wordpress/wp-content/uploads/2013/02/mayko-summary.png)



The plugin integrates directly into the vSphere Web Client, and I hope to see this feature installed by default in future versions of the Web Client.


## A small suggestion:


I hope that in future versions of Makyo, you could use the same tool to copy VMs or vApps from standalone ESXi hosts into your vCenter infrastructure. Today you can use VMware Workstation to move and copy between local to Workstation storage, standalone ESXi hosts and vCenter managed infrastructure and  adding the same kind of feature set to the Web Client would be great.

It would make it easier to migrate from small test environments to live production environments, all inside the vSphere Web Client.
