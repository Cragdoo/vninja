---
author: cmohn
comments: true
date: 2013-01-16 21:59:07+00:00
layout: post
link: http://vninja.net/vmware-2/vcenter-operations-manager-vcenter-deployment-dependency/
slug: vcenter-operations-manager-vcenter-deployment-dependency
title: vCenter Operations Manager and vCenter Deployment Dependency
wordpress_id: 2351
categories:
- VMware
tags:
- vCenter
- vCenter Operations Manager
- Virtualization
- VMware
- vSphere
- workaround
---

In order to be able to deploy vCenter Operations Manager, the ESXi host it is deployed to needs to be managed by a vCenter. At first glance, that seems like a fair and pretty straight forward requirement, right? But, is it?

While I can see the need for a vCenter in the environment that vCenter Operations Manager is supposed to monitor, I don´t understand why the host you want to deploy the vApp on has to be managed by a vCenter instance as well?

I encountered this exact scenario at a client site today; **What do you do if you want to deploy vCenter Operations Manager on a standalone ESXi host, and have it monitor a production vSphere environment, which has a running vCenter installation?**

The [vCenter Operations Manager 5 Deployment and Configuration Guide ](http://www.vmware.com/pdf/vcops-5-installation-guide.pdf) clearly states that in order to deploy and run the vCenter Operations vApp vCenter Server 4.0 U2 or later is required:


<blockquote>**vCenter Server and ESX Requirements:**
The vCenter Operations Manager vApp requires the following vSphere environment.
vCenter Operations Manager is compatible with:

> 
> 
	
>   * System that serves as the target of data collection: VMware vCenter Server 4.0 U2 or later
> 
	
>   * System running the vApp: VMware vCenter Server 4.0 U2 or later
> 
	
>   * Host running the vApp: ESX/ESXi 4.0 or higher
> 

</blockquote>


And in the **Deploy the vCenter Operations Manager vApp** section of the same document:


<blockquote>

> 
> 
	
>   * Do not deploy vCenter Operations from an ESX host. Deploy only from vCenter Server.
> 

</blockquote>


The reason why the client wanted to deploy vCenter Operations on a standalone, unmanaged host, was pretty simple; They are experiencing some rather serious performance issues, and wanted to use vCenter Operations Manager to help identify the bottlenecks and document them, but did not want vCenter Operations Manager itself to influence the performance of the production cluster(s).




I was able to "work around" the issue by temporarily setting up a vCSA, in trial mode, register the ESXi host, deploy the vCenter Operations Manager vApp to the new vCSA, configure it and then subsequently shut it down again. In fact, there is even an option in the configuration to not monitor the vCenter instance it´s deployed on, so someone else must have had some mildly similar thoughts at one point.

While this will work, its not ideal having to go through loops like this to deploy a management solution. I´ll happily admit that this is a fringe case, and not the most common deployment scenario, but I would still like to see VMware do something about the vCenter requirement for the host the vApp is deployed on.

**Requiring vCenter for the hosts and cluster it´s monitoring is fine, and expected, but why does the host it´s deployed on require it?**






