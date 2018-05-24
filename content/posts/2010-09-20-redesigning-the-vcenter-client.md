---
author: cmohn
comments: true
date: 2010-09-20 22:12:48+00:00
layout: post
slug: redesigning-the-vcenter-client
title: Redesigning the vCenter Client?
url: /virtualization/redesigning-the-vcenter-client/
wordpress_id: 264
categories:
- Virtualization
tags:
- Ops
- vCenter
- Virtualization
- VMware
- vSphere
---

![](/images/logos/vmware-logo.gif)In fresh blog post, called "[Resource pools and simultaneous vMotions"](http://frankdenneman.nl/2010/09/resource-pools-and-simultaneous-vmotions/) by [Frank Denneman](http://twitter.com/FrankDenneman) prompted a quick Twitter discussion regarding the vCenter client (and perhaps even vCenter itself). A simple


<blockquote>Why are there no folders under Host and Clusters view ?</blockquote>


from [Maish Saidel-Keesing](http://twitter.com/maishsk/status/25056375559) got the ball rolling.

Could it be that the design of the client itself helps perpetuate the myth the resource pools is an organizational unit, one that should be used as a way of grouping VMs? As Frank says;


<blockquote>This is not (the) reason why VMware invented resource pools.</blockquote>


I'm not going to get into why this is a bad idea, both [Frank](http://twitter.com/FrankDenneman) and [Duncan](http://twitter.com/DuncanYB) will have far more intelligent feedback to you if you are interested in discussing this.

**Now, if you could redesign the vSphere Client, based on your own experience, what would you change?** 

I can see that in many cases, a lightweight vCenter Simple Mode client would do the trick. Give your VM admins the Simple Mode client, and they won't have to worry about resource pools, HA/DRS and other advanced features. Let them administer the VMs, like adding networking, powering on/off etc. The same applies for your storage admins. Give them a small, limited, client that only allows them to configure storage aspects.

I know you can do most of this with permissions etc, but I still feel a limited client could be _one_ way to go.

In other cases, like mine, it won't help splitting up the client into smaller chunks. As in most SMBs, I wear a _lot_ of different hats during a normal working day. I'm the networking-/storage-/operating-systems-guy in a small shop, where there simply isn't room for delegating all these tasks to specialized teams or even other admins.

**Perhaps dividing the client into specialized sub-topics would help?** 

A task based user experience where you can select between **"VM Operations"**, **"vSphere Operations"**, **"Storage Operations"** or **"Network Operations"** as your initial choice, and then limit what you can configure based on your initial choice would help organize the screen real estate? You could also have sub-sections like **"Monitoring VMs"**, **"Monitoring Storage"** and so on, displaying a performance overview as the initial point of entry.

You could still have an **"Advanced Mode"** that works the same way the vCenter Client works today, but the default would be a simplified experience that is based on the task you have at hand.

_Am I completely out on a limb here, or is this semi-rant making some sense, somewhere? What would you change, and how?_
