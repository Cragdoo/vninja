---
author: Christian Mohn
comments: true
date: 2016-11-23 17:37:07+00:00
layout: post
slug: got-vmware-vrealize-log-insight
title: Got VMware vRealize Log Insight?
url: /virtualization/got-vmware-vrealize-log-insight/
wordpress_id: 4416
categories:
- Virtualization
tags:
- Free
- Log Insight
- Software
- VMware
---

Recent conversations with existing and potential clients made me realize that many are not aware that they most likely are entitled to use [VMware vRealize Log Insight](http://www.vmware.com/products/vrealize-log-insight.html) in their environment. _For free._

Back in March 2016 VMware announced that everyone with a valid VMware vCenter license are also entitled to  25-OSI pack of vRealize Log Insight for vCenter Server. This means that you can gather logs for up to 25 ESXi hosts, VMs or other devices, in your environment.
<!--more-->

It even allows you to use the following VMware content packs (3rd party content packs requires a full Log Insight license)





  * Horizon View


  * NSX


  * OpenStack


  * vCenter Operations Manager


  * vCloud Automation Center


  * vCloud Director


  * vCNS


  * Virtual SAN


  * vRealize Automation


  * vRealize Operations Manager


  * vSphere



This should be a no-brainer for everyone with a _>24 host_ VMware vSphere environment, the value that vRealize Log Insight provides is enormous, especially when it comes to troubleshooting. Why 24 when you get a 25 pack you may ask? Well, you'll want to use one of them for vCenter itself, leaving 24 for your ESXi hosts. If your environment is smaller than 24 hosts, the remaining OSI's can be used to monitor just about anything that can log to a syslog service, like switches, routers and other devices.

[caption id="attachment_4417" align="aligncenter" width="620"]![Credits: http://www.gratisography.com](http://vninja.net/wordpress/wp-content/uploads/2016/11/116H-1024x683.jpg) Credits: [gratisography.com](http://www.gratisography.com)[/caption]

So, if you're not running it already, logon to [my.vmware.com](https://my.vmware.com) and download it today — you can then use your existing vCenter license to activate it. You'll be drinking from the log-hose in no time.





#### #vDM30in30 progress:
[progressbar_circle percent= 53]
