---
author: cmohn
comments: true
date: 2014-06-08 22:24:23+00:00
layout: post
slug: marvin-speculation
title: Some more Marvin Speculation
url: /vmware-2/marvin-speculation/
wordpress_id: 3212
categories:
- VMware
tags:
- Hyper Converged
- Marvin
- SDDC
- Speculation
- VMware
---

[caption id="attachment_3213" align="alignright" width="121"]![Marvin the Paranoid Android (HHGG)](http://vninja.net/wordpress/wp-content/uploads/2014/06/Marvin_HHGG.jpg) Marvin the Paranoid Android (HHGG)[/caption]

After close to a full day of Twitter speculations and discussions on what [Marvin really is](http://vninja.net/vmware-2/vmwares-mystic-marvin-project/), some thoughts kept stuck with me, and this is my attempt at articulating what I think VMware might be up to.

_Please note that I have no real knowledge of the status of the project, nor if it really exists outside of a poster in a window in the VMware campus. All I do know is that there is a [trademark registered](http://trademarks.justia.com/861/60/marvin-86160864.html), and that there seems to be some merit behind the speculations done by [CRN](30/project-mystics-potential-competitors-to-vmware-bring-it-on.htm) and [The Register](http://www.theregister.co.uk/2014/03/19/emc_sailing_up_the_mystic_river/)._

There is one little tidbit in the trademark registration that has caused more than a few people to raise their eyebrows:



<blockquote>Computer hardware for virtualization; computer hardware enabling users to manage virtual computing resources that include networking and data storage</blockquote>



That's right, it mentions hardware! Does that mean that VMware is going to start building their own hardware, or OEM it from someone? I really don't think that is the case. As far as I can gather, Marvin is more than a single physical hyper-converged hardware platform, with known VMware bits on top, sprinkled with some new management interface. Pinning this appliance model to a single vendor, like [Nutanix](http://nutanix.com) does with their [Super Micro chassis and components](http://download.nutanix.com/guides/c_3_5/xhtml/oxy_ex-1/topics/hardware/system_specifications_nx7000_r.html), is not how VMware usually operates. I don't even think it's part of VMware's DNA to limit Marvin to a single hardware vendor, even if EMC is their biggest stock holder. I think the key here is to think of **Marvin as a standard for building hyper-converged solutions**. Build nodes to spec, and VMware will certify them for Marvin. Imagine that? You can probably mix and match nodes from different vendors, in the same "MarvinMatrix".

We've already seen [VSAN Ready Nodes](http://blogs.vmware.com/vsphere/2014/03/virtual-san-ready-nodes-cisco-dell-fujitsu-ibm-super-micro-next-30-days.html), why not build **Marvin Ready Nodes** (for some reason I think VMware marketing might just come up with a better term come launch time...). Dell and Super Micro seem likely candidates, but how about Intel nodes? I am fairly sure that there are [people](http://en.wikipedia.org/wiki/Pat_Gelsinger) within VMware, and [The Federation](http://emcfederation.com), that has pretty good ties in that direction.

The possibilities here are pretty awesome, just like they are with existing hyper-converged solutions. Need more compute? Stick in another node into your network "matrix", import it and it auto-configures itself based on whatever policy you have active. Need more storage, well, do the same thing. Add a new node, or just stick in a few more flash or hard drives into your existing chassis.

We all know this is going to be based on vSphere and VSAN, but there are other components that are yet left open for speculation:

**What about networking?** We know VMware is pushing NSX, and we know they are working on making a smaller scale installation easier to implement. Given the time-frame we are looking at, namely next summer (read VMworld), I doubt that NSX will be a part of the first revision. Perhaps it's going to be [Arista](http://www.arista.com/en/) at that point?

The second thing is the magic sauce that will tie everything together, and I have a feeling that this is where VMware's Marvin team members is really focusing its attention these days.  A solution like this, if my speculations are right, really needs a good management solution if it is to really deliver on what it seems to promise, namely a Software Defined Data Center, in a box. Or two boxes, for redundancy of course, or given VMware's normal reliance on N+1, it's probably best if you buy three of them to begin with.



<blockquote>_Why should I want to make anything up? Life's bad enough as it is without wanting to invent anymore of it._  
- Marvin, the Paranoid Android</blockquote>
