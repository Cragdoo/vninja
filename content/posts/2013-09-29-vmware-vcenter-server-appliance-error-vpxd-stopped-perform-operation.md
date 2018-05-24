---
author: cmohn
comments: true
date: 2013-09-29 23:16:25+00:00
layout: post
link: http://vninja.net/vmware-2/vmware-vcenter-server-appliance-error-vpxd-stopped-perform-operation/
slug: vmware-vcenter-server-appliance-error-vpxd-stopped-perform-operation
title: 'VMware vCenter Server Appliance Error: VPXD must be stopped to perform this
  operation'
wordpress_id: 2751
categories:
- VMware
tags:
- '5.5'
- ESXi
- vCenter
- vCenter Server Appliance
- VMware
- vSphere
---

While deploying a fresh vCenter Server 5.5 Appliance, I ran into an issue getting it configured.

When the appliance is deployed, the first time you log in you get presented with the configuration wizard. _The wizard clearly states that if you want to set a static ip, or hostname, you should cancel the wizard, do the network configuration and then re-run the wizard after the fact._

Well, that´s what I did, and it resulted in the following error when trying to create the embedded database:

[cc lang="bash" width="100%" nowrap="0"]
VC_CFG_RESULT=410(Error: VPXD must be stopped to perform this operation.)
[/cc]

I even tried redeploying the appliance from scratch, but sadly that had the same outcome.

In the end, I was able to complete the configuration by opening an SSH session to the vCenter appliance, and running the following command to stop the vmware-vpxd service mentioned in the error message:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
~ # service vmware-vpxd stop
[/cc]

After that I could successfully complete the Setup Wizard. Hopefully this will help someone finding themselves in the same conundrum in the future.


### Update:


_Since my setup is a single host, and at the moment of deploying the vCenter Server Appliance there was no existing vCenter in place, I deployed the appliance directly to an ESXi host. When you do this, you do not get the OVF deployment wizard that asks your IP adresses, netmasks etc. I suspect that this is the root cause of this issue, and that this is something you can/will run into of you deploy it in this manner._
