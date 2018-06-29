---
author: cmohn
comments: true
date: 2013-02-05 12:26:26+00:00
layout: post
slug: purging-vcenter-operations-manager-5-x-database
title: Purging the vCenter Operations Manager 5.x Database
url: /vmware-2/purging-vcenter-operations-manager-5-x-database/
wordpress_id: 2368
categories:
- VMware
tags:
- vCenter
- vCenter Operations Manager
- Virtualization
- VMware
---

A client of mine recently had their vCenter 5.0 appliance replaced with a new vCenter 5.1 appliance, something vCenter Operations Manager did not enjoy. In this case the original vCenter 5.0 Appliance was turned shut down, a new 5.1 Appliance deployed, with the same IP as the old one,  and then configured. Naturally the existing hosts were added to the fresh vCenter, clusters recreated and in general everything was reconfigured.

Why a simple [upgrade from 5.0 to 5.1](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2033990) wasn't performed is beyond me, but the end result was that the existing vCenter Operations Manager I had deployed in this environment was no longer able to monitor the environment.

Simply removing the registration of the old vCenter and replacing it with the new instance was not trivial, as this generated an exception in the vCenter Operations Manager admin interface:



<blockquote>Exception occurred during vCenter Server registration. UUID is not available for the currently registered vCenter ([https://<vcenter](https://%3cvcenter) FQDN>/sdk). This can happen if you added the vCenter from custom UI instead of the admin UI. Please contact support to fix the problem and then attempt to register a new vCenter.</blockquote>



Trying to force the registration from the command line on the vCenter Operations Manager UI VM using the following command also failed, with the same error message:

[cc lang="bash" width="95%" theme="blackboard" nowrap="0"]
vcops-admin register update --vc-name [vc-name] --vc-server [https://vc-server/sdk] --username [vc-username] --password [vc-password] --force
[/cc]

Naturally the issue is caused by the non-standard "upgrade" performed on the vCenter, but the end result that I no longer had old performance data available anywhere. The vCenter database was no longer available, since no actual upgrade was performed and the old vCenter had been removed from vCenter Operations Manager.

My first thought was to simply delete and re-deploy vCenter Operations Manager, but since I was working remotely this wasn't really a viable option, considering that the OVF package was located on my local machine and not in the clients network

After some playing around to try to avoid deploying again over a slow WAN link, I discovered a somewhat undocumented parameter for the _vcops-admin (run on the UI VM command line)_ command.

[cc lang="bash" width="95%" theme="blackboard" nowrap="0"]
vcops-admin purge --vc-name
[/cc]

This parameter is not listed in the man pages for the _vcops-admin_ command, and what it does is to completely purge a previously registered vCenter. And by purge, it really means purge. **All traces of the old vCenter data is removed from the vCenter Operations Manager database, any old performance data and metrics are completely removed.**

In my case this was exactly what I was looking for, since the alternative was to redeploy the whole vCenter Operations Manager package.

**Normally this is not something you want to do, it would be preferable to figure out the root cause of your issues instead of purging your existing database and lose historic data, but in some cases this can come in handy. **

One example would be if you are playing around with vCenter Operations Manager in your lab environment, this is a quick and easy way to start over instead of re-deploying.
