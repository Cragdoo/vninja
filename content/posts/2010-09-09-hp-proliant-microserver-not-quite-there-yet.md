---
author: cmohn
comments: true
date: 2010-09-09 22:44:25+00:00
layout: post
link: http://vninja.net/news/hp-proliant-microserver-not-quite-there-yet/
slug: hp-proliant-microserver-not-quite-there-yet
title: HP ProLiant MicroServer - Not Quite There Yet?
wordpress_id: 234
categories:
- Hardware
- News
- Virtualization
---

![](/images/logos/hp.png)When HP announced their new [ProLiant MicroServer](http://h10010.www1.hp.com/wwpc/us/en/sm/WF05a/15351-15351-4237916-4237918-4237917-4248009.html), I really hoped that it would be the perfect answer to a specific use case I've been looking at lately.

Basically, what I'm looking for is a small chassis, low noise branch office server that would be used to host a single virtual machine, offering [Read-Only Domain Controller (RODC)](http://technet.microsoft.com/en-us/library/cc732801%28WS.10%29.aspx) and [Distributed File System (DFS)](http://en.wikipedia.org/wiki/Distributed_File_System_%28Microsoft%29) file-shares.

Initially it looked to fit the bill perfectly: 


  * Small footprint; Check


  * Low Noise levels; Check



But, sadly, that's where it stops. The first version of the HP ProLiant MicroServer comes with one CPU offering, namely theAMD Athlon II NEO N36L which isn't all that much to run even a single-VM ESXi instance on. 

The current tech spec page does not go into much details about the storage controller, other than it's an "Integrated 4 port SATA RAID Storage Controller", which makes it impossible to check for compatibility on the official [VMware HCL](http://www.vmware.com/resources/compatibility/search.php).

The 1GbE NC107i NIC supplied with the server, seems to be supported by VMware though, at least according to the [ProLiant option VMware support matrix](http://h20338.www2.hp.com/enterprise/cache/525605-0-0-0-121.html).

I understand that HP created this server for a different use-case than the one I'm outlining here, and you can't really criticize them for that, I just hope that this is just the first of several offerings from HP and that the next version comes with better CPU offerings. 

A proper CPU would make this baby the perfect entry level, small footprint, low noise branch-office server.

**Update:** [Simon Seagrave](http://twitter.com/kiwi_si) has posted as _"somewhat"_ more verbose analysis of the HP ProLiant Microserver: [New HP Proliant MicroServer – a decent vSphere lab server candidate?](http://www.techhead.co.uk/new-hp-proliant-microserver-a-decent-vsphere-lab-server-candidate). His conclusion is pretty much the same as mine though; give us more CPU and a vSphere supported RAID controller and we're all set. I couldn't agree more.



