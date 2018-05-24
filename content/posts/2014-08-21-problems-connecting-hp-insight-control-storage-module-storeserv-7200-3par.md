---
author: cmohn
comments: true
date: 2014-08-21 22:21:59+00:00
layout: post
link: http://vninja.net/storage-2/problems-connecting-hp-insight-control-storage-module-storeserv-7200-3par/
slug: problems-connecting-hp-insight-control-storage-module-storeserv-7200-3par
title: Problems connecting HP Insight Control Storage Module to StoreServ 7200 (3Par)
wordpress_id: 3336
categories:
- Storage
tags:
- 3Par
- HP
- HP Insight Control
- StoreServ
- Troubleshooting
- vCenter Operations Manager
---

A customer of mine, who runs a pure HP environment based on c7000 and StoreServ 7200, wanted to get the [HP Insight Control Storage Module for vCenter](http://www.youtube.com/watch?v=A93zVmnheEE) up and running. The problem was that while we were able to connect to the older MSA array they run for non-production workloads, we were unable to connect to the newer StoreServ 7200. There is full IP connectivity between the application server that the HP Insight components run on and the storage controllers/VSP (no firewalls between them, they are located in the same subnet).

The only error message we got was an “unable to connect” message, when using the same credentials and ip address used for the 3Par Management Console. After reaching out to quite a few people, including [Twitter](https://twitter.com/h0bbel/status/499488699412123649), we finally found the solution. It turns out that the CIM service on the array was not responding, in fact it was disabled, which naturally resulted in not being able to connect.

A quick ssh session to the array, revealed that the CIM service was disabled. 
[cc lang="bash" width="100%" theme="blackboard" nowrap="1"]
login as: username
Password: *************

3par-array cli% showcim
-Service- -State-- --SLP-- SLPPort -HTTP-- HTTPPort -HTTPS- HTTPSPort PGVer CIMVer
Disabled  Inactive Enabled     427 Enabled     5988 Enabled      5989 2.9.1 3.1.2
3par-array cli% stopcim
Are you sure you want to stop CIM server?
select q=quit y=yes n=no: y
CIM server stopped successfully.
3par-array cli% startcim
CIM server will start in about 90 seconds
3par-array cli%
[/cc]

Restarting it fixed the issue, and we now have StoreServ data available directly in the vSphere Web (and C#) client. This also fixed the connection problem we had with vCenter Operations Manager and the [The HP StoreFront Analytics adapter](http://h20392.www2.hp.com/portal/swdepot/displayProductInfo.do?productNumber=vCOPS).

_So, if you are unable to connect to your StoreServ, check the CIM service - It might just be disabled._
