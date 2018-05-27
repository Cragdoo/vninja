---
author: cmohn
comments: true
date: 2017-04-11 16:08:30+00:00
layout: post
slug: vmware-vsan-6-6-whats-new-for-day-2-operations
title: VMware vSAN 6.6 - What's New for Day 2 Operations?
url: /virtualization/vmware-vsan-6-6-whats-new-for-day-2-operations/
wordpress_id: 4635
categories:
- Virtualization
tags:
- Day 2
- management
- News
- Operations
- VMware
- VSAN
---

VMware has just [announced vSAN v6.6](https://blogs.vmware.com/virtualblocks/2017/04/11/whats-new-vmware-vsan-6-6/), with over 20 new features. While new and shiny features are nice I'd like to highlight **a couple** that I think might be undervalued from release feature-set perspective, but highly valuable in day to day operations of a vSAN environment, otherwise known as _Day 2 operations_.

**vSAN Configuration Assist** is one such new feature. While it's true that it helps first time configuration of a greenfield installation with vSAN (no more [bootstrapping](https://vdc-download.vmware.com/vmwb-repository/dcr-public/b5a7ed3b-388b-4baf-9345-f3cc675efa26/8f9e7e36-c473-484a-a762-462e2c557992/VSANTechnote_BootstrappingVSAN_final.pdf), yay!), it also helps with Day 2 operations.

It helps configure new hosts added to an existing vSAN enabled cluster, but it also makes it possible to automate updating of IO controllers, both firmware and drivers directly from within vCenter. As everyone should know by now, vSAN is highly dependent on drivers and firmware being on supported levels. This improvement helps the improved vSAN Health Check (Enhanced Health Monitoring) [![](/img/vsan-host-health-check-300x177.png)](/img/vsan-host-health-check.png)alert you when new and _verified _drivers/firmware are available, and if the controller tools are available on the ESXi host, it can also update the firmware for you.

Directly from vCenter, utilising maintenance mode like you're used to from patching your ESXi hosts from VMware Update Manager (even if it's _not_ integrated into VUM at this point).

This new features takes the vSAN HCL list to a new level, it's no longer just a list over supported IO controllers and their firmware and drivers, it's a now also a software distribution point. At the moment **Dell EMC**, **Fujitsu**, **Lenovo** and **SuperMicro** are all supported vendors for this new distribution model, hopefully the rest will follow suit quickly.

The second feature I would like to highlight as a Day 2 operations enhancement, is the new **vSAN Cloud Analytics** feature. If you participate, in the Customer Experience Improvement Program (CEIP), it will enable custom alerting for **your environment**, based on your own environment. For instance, if a new Knowledge Base article is published, that pertain to your specific setup, it will alert you about it. One example might be if you have Intel X710 NICs, [which can cause PSODs](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2126909) — Wouldn't it be nice if you got alerted that this might be an issue, and then told how to remediate it? Well, with vSAN 6.6 you'll get exactly that.

With vSAN 6.6 you get both automated, and verified, firmware/driver upgrades, as well as proactive alerting for potential issues through the _hive-mind_ that is the analytics service. This is what VMware calls **Intelligent Operations and Lifecycle Management **in this release, and it's really hard to argue with that.

Of course, vSAN 6.6 provide other Day 2 Operations enhancements as well, like _Degraded Device Handling (DDH)_, _Simplified_ _Stretched Cluster Witness replacement procedures_, _Capacity and Policy Pre-Checks_ and access to the vSAN control plane trough the _ESXi Host Client_, but I'll leave those for later posts.
