---
author: cmohn
comments: true
date: 2011-01-17 08:37:37+00:00
layout: post
slug: hp-proliant-microserver-%e2%80%93-oh-its-there
title: HP ProLiant MicroServer – Oh, it's there!
url: /news/hp-proliant-microserver-%e2%80%93-oh-its-there/
wordpress_id: 785
categories:
- Hardware
- News
---

![](/images/logos/hp.png)In a previous article, [HP ProLiant MicroServer – Not Quite There Yet?](http://vninja.net/news/hp-proliant-microserver-not-quite-there-yet/), I questioned whether the HP Proliant MicroServer could have a place in a corporate environment. 

I finally got my hands on one, and for my specific use case it's pretty much perfect! Sure, I would love to have some vSphere supported RAID controller and a beefier CPU but in my use case the MicroServer delivers just what we need for some of our small branch offices.

The MicroServer is very quiet (22 dB), has a  very small footprint (21 x 26 x 26,7cm), that fits into a office environment (no datacenter) and you can easily fit a [read-only domain controller (RODC)](http://technet.microsoft.com/en-us/library/cc732801%28WS.10%29.aspx) on it.

Our setup ended up with the following to support a remote office with 4-5 people.




[![HP MicroServer](http://farm6.static.flickr.com/5244/5362684549_6b4b6dca8f_m.jpg)](http://www.flickr.com/photos/h0bbel/5362684549/)

  * HP MicroServer with 8GB ram and HP MicroServer Remote Access Card


  * Iomega IX4-200d for backup purposes


  * Watchguard x10e Firewall


  * HP Procurve 1410-8g


  * HP ProCurve MSM310 Wireless Access Point


  

The MicroServer is installed with VMware vSphere Hypervisor (ESXi) 4.1, running one VM with Windows Server 2008 R2 configured as a RODC. In addition to the domain controller features, it acts as a local file server replicating via DFS. 

Why run it in vSphere at all? Well, I do like the portability of virtual machines and it does make us hardware independent in the sense that we could change the physical hardware around without having to reinstall the Windows Server. It would also make it a lot easier to perform remote backups of the VM itself, should we want to do so.

All in all, the MicroServer does indeed fit into some corporate environments and scenarios. I would still not use it for a vSphere lab, unless I can crab a couple of them and set up a cluster though. The MicroServer is so small that the WAF index might just make that plausible!


