---
author: cmohn
comments: true
date: 2010-10-15 20:29:14+00:00
layout: post
link: http://vninja.net/virtualization/developer-meets-powercli-awesomeness-ensues/
slug: developer-meets-powercli-awesomeness-ensues
title: Developer meets PowerCLI - awesomeness ensues
wordpress_id: 397
categories:
- Virtualization
tags:
- Ops
- PowerCLI
- PowerGui
- Powershell
- vCenter
- Virtualization
- VMware
- vSphere
---

![](/images/logos/PowerCLI.png)
A couple of days ago, while I was at VMworld Europe I got the following [tweet](http://twitter.com/neslekkim/status/27125696800) from [Asbjørn A. Mikkelsen (@neslekkim)](http://twitter.com/neslekkim) (translated from norwegian):


<blockquote>@h0bbel Do you know if I can script something against vCenter to duplicate (or create from template) VMs, and also start/stop them?</blockquote>


My immediate response, was of course to suggest using [PowerCLI](http://communities.vmware.com/community/vmtn/vsphere/automationtools/powercli). Asbjørn, who works as a full time developer, jumped at PowerCLI immediately and within a very short time frame came up with a PowerCLI script for the task at hand.

You can [download the script](/code/VI-automation1.ps) and play around with it, if you want. Inline documentation is in Norwegian, and if Asbjørn had intended to redistribute the script I'm sure he would have optimized it more than the current revision.
<!-- more -->
The point here isn't the implementation itself, but rather the fact that he was able to put this automation routine together very quickly, and completely without prior knowledge to PowerCLI at all. 

 I do feel bad for not pointing him to [PowerGUI](http://www.powergui.org/index.jspa) and the [VI Toolkit PowerPack](http://www.powergui.org/entry.jspa?externalID=1802&categoryID=290) before he sat down and crafted this in Notepad though. I'm sure he cursed me silently as soon as I pointed them out to him, as that combination makes this kind of automation so much easier.
