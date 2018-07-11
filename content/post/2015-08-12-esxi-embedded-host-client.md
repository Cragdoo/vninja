---
author: cmohn
comments: true
date: 2015-08-12 16:11:16+00:00
layout: post
slug: esxi-embedded-host-client
title: ESXi Embedded Host Client
url: /vmware-2/esxi-embedded-host-client/
wordpress_id: 3714
categories:
- VMware
tags:
- Awesome
- VMware
- vSphere
- Web Client
---

I almost choked on my coffee this morning when I saw [William Lam announcing a new VMware Fling](http://www.virtuallyghetto.com/2015/08/new-html5-embedded-host-client-for-esxi.html) called [ESXi Embedded Host Client](https://labs.vmware.com/flings/esxi-embedded-host-client). Finally the day when we can get a local vSphere Web Client on a standalone host is here, and it's not a moment too soon. This feature has been missing since ESX 3 and it's VMware Infrastructure Web Access. For now, this is a Fling (which means unsupported and so on), but I really hope that this ends up being built-in to ESXi very soon -- even on the free vSphere Hypervisor.
<!--more-->

So, what does it look like? Well, to be honest it looks pretty darn awesome, and you know what? No Flash! Yes, it's HTML5 baby!

![VMware ESXi Host Client](/img/VMware-ESXi-Host-Client-2015-08-12-17-49-38-1024x415.png)

If you are running ESXi 6, installation is as easy as installing a .**vib **(in ESXi 5.5 there is a workaround to get it running, highlighted in [Williams post](http://www.virtuallyghetto.com/2015/08/new-html5-embedded-host-client-for-esxi.html)). So, give it a spin and make sure to provide feedback on the [Flings site](https://labs.vmware.com/flings/esxi-embedded-host-client) to help ensure VMware sees that we as users of their products want this feature to be core to vSphere in upcoming releases. Make your voice heard, and make sure the team who is behind this get the credit they deserve.
