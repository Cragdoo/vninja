---
author: Christian Mohn
comments: true
date: 2013-09-30 12:20:38+00:00
layout: post
slug: upgrading-vsphere-vcenter-appliance-5-1-5-5
title: Upgrading vSphere vCenter Appliance 5.1 to 5.5
url: /vmware-2/upgrading-vsphere-vcenter-appliance-5-1-5-5/
wordpress_id: 2762
categories:
- VMware
tags:
- Appliance
- Upgrade
- vCenter
- VCSA
- vSphere
- vSphere 5.5
---

In [VMware KB Upgrading vCenter Server Appliance 5.0.x/5.1 to 5.5 (2058441)](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2058441) the procedure for upgrading an existing 5.0/5.1 vSphere vCenter Server Appliance is outlined, walking you through the steps required including deploying a new 5.5 vCSA and transferring the data from the old instance to the new one. Straight forward procedure, but there is one small caveat in this process.

One important thing to remember, and something I don´t feel that the knowledge-base article highlights well enough is that the new v5.5 appliance should not be configured in any way when deployed.

In my opinion there should be a new step between steps 1 and 2 in the KB article, detailing that the default blank values should not be changed at all.

[![vCenterApplianceUpgrade-1](/img/vCenterApplianceUpgrade-1-150x150.png)](/img/vCenterApplianceUpgrade-1.png)
_When using the "Deploy OVF" wizard in the Web Client or Desktop Client, you get asked for network details like IP, subnet, gateway and hostname. In order for the new appliance to successfully import your old data, and take over the old appliance details like hostname, IP adress and so forth, all these values should be left blank while importing to ensure a successful upgrade._

If you do this, the upgrade succeeds as anticipated, and the old vCenter Server Appliance is powered down and the new one takes over.
