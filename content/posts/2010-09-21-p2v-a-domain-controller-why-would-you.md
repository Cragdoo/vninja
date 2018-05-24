---
author: cmohn
comments: true
date: 2010-09-21 19:39:07+00:00
layout: post
link: http://vninja.net/virtualization/p2v-a-domain-controller-why-would-you/
slug: p2v-a-domain-controller-why-would-you
title: P2V a Domain Controller? Why would you?
wordpress_id: 279
categories:
- Virtualization
tags:
- Maintenance
- Ops
- Virtualization
---

![](/images/logos/vmware-logo.gif)Some topics seem to pop up at random intervals, one of them being virtualizing Microsoft Active Directory Domain Controller servers. The question is often either "_Should I virtualize my Domain Controllers, and if so should I virtualize all of them?_" or "_Should I do a P2V (Physical 2 Virtual) conversion of my existing Domain Controllers, or create new ones?_"

In this post, I'll be talking about the second question. While there is a lot to be said about the first one as well, I'll leave that for future post. 

Most businesses have an existing Active Directory when they decide to virtualize. There might be different reasons for going virtual with regards to Active Directory, but in my mind there are close to no scenarios where I would even consider doing a P2V conversion of an Domain Controller. 

The reasons for this are plenty:




  * **You need to do a _cold_ conversion**  
You absolutely should not do a hot P2V migration of a DC. If you try to hot migration, you will end up with a domain controller that is out of sync with the others, lots of issues and a really painful headache


  * **Never power on the old server**  
The old server, the one you did a cold P2V migration of, must _never_ be powered back on after the new virtual instance is started. If it gets powered back on, you will once again be in a world of hurt.


  * **Potential Cleanup problems**  
You need to clean up the old driver stack (most P2V tools will do this for you), and you might end up with for instance two network cards that share the same IP, one of them hidden from view and not very easily removed. This could in turn make the DNS services on a converted domain controller does bind to the wrong network interface. And we all know what happens to Active Directory if DNS doesn't work right.



I'm sure there are many other potential issues as well, like Kerberos authentication or trust failures and so on. This is not a situation you want to end up in, especially not in your production environment.

[Gabrie van Zanten](http://twitter.com/gabvirtualworld) recently published a recipe for P2V migrations of existing Domain Controllers, called [Virtualizing a domain controller, how hard can it be?](http://www.gabesvirtualworld.com/virtualizing-a-domain-controller-how-hard-can-it-be/) and I'm confident that this method would probably work out fine. 

My question is this; _Why would you want to do this in the first place?_ It's not like it's hard to set up a new Domain Controller, make sure it replicates properly with the existing physical or virtual ones, transfer any FSMO roles the soon-to-be-decommissioned Domain Controller has to the new instance and then safely and timely remove Active Directory from the old server. 

Of course, Gabe has a point when he mentions that the issues you might get with a botched P2V of a Domain Controller would be the same as old style bad management like using Symantec Ghost on a DC and roll back to an old image if something fails, but why risk it at all? 

Deploying a new Windows Server 2008 R2 VM, running _dcpromo_ and setting up DNS does not take a long time, nor is it very complex to do. 

I have not timed this, but I seriously doubt that creating a cold P2V migration boot device, shutting down the physical server, booting the cold migration tool, do the actual P2V conversion and powering on the new VM takes less time than it takes to set up a new VM. You might argue that you will have to install anti-virus and backup agents and possibly other tools to the new VM as well, but if your infrastructure is somewhat reasonably set up with automation tools etc. this should not really be a factor to consider. Besides, if you do it this way you have a return path, after all you haven't removed anything!

In fact, I'm pretty sure this whole post took longer to write than it would take to actually set up a new Domain Controller in my production environment. 

My conclusion is, don't bother risking a P2V of a Domain Controller. Set up a new VM instead, **it's easy, quick and risk free**. 

In other words, as the [vSensei](http://twitter.com/search?q=vsensei) would say ["just because you can, no mean you should"](http://twitter.com/ChrisDearden/statuses/25135153842)

**So [Gabe](http://twitter.com/gabvirtualworld), as far as this one goes; You're on your own! ;-)**
