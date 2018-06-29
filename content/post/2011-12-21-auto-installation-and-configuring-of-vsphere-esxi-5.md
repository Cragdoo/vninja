---
author: cmohn
comments: true
date: 2011-12-21 23:36:19+00:00
layout: post
slug: auto-installation-and-configuring-of-vsphere-esxi-5
title: Auto Installation and Configuring of vSphere ESXi 5
url: /virtualization/auto-installation-and-configuring-of-vsphere-esxi-5/
wordpress_id: 1658
categories:
- Virtualization
- VMware
tags:
- automation
- ESXi
- fun
- Ops
- PowerCLI
- Powershell
- realworld
- Virtualization
- VMware
---

One of the last projects I've been involved with at Seatrans, is to automate the installation and configuration of vSphere ESXi 5 hosts for deployment on vessels. I've talked a bit about this before, both on [vSoup](http://vninja.net/virtualization/blatant-self-promotion/) and in [Setting Up Automated ESXi Deployments](http://vninja.net/virtualization/setting-up-automated-esxi-deployments/) where I outlined my PXE and PowerCLI based installation and configuration scheme. Not much has changed since then, except updating the PXE server to offer ESXi 5, instead of ESXi 4 and a lot of work has been put into the scripting, including a front-end GUI for the PowerCLI script itself. The end "product" is now in place for mass deployments for internal use.

The following video shows how the PXE based installation works, as well as a run through the now GUI based configuration tool aptly called _Seatrans Hypervisor Installation Tool_. 

The video jumps a bit between two VMs, one running Windows Server 2008 R2, that runs the DHCP/PXE services and the PowerCLI script, and one that gets ESXi installed and configured:



This goes to show that you can create your own, specialized and portable deployment solution without requiring elaborate network configurations or reconfiguring of existing infrastructure.

**Note:** I will not be providing downloadable versions of the final script at this time. The reason for this is quite simple, it's very specific and tailored for a non-generic environment. If I can manage to find the time, I'll post a generic version later but in order for anyone else to utilize the PowerCLI scripts I've created, a lot of work is required. 

