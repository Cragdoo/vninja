---
author: Christian Mohn
comments: true
date: 2017-02-25 08:50:26+00:00
layout: post
slug: vmware-vsphere-6-5-psod-gp-exception-13
title: 'VMware vSphere 6.5 PSOD: GP Exception 13'
url: /vmware-2/vmware-vsphere-6-5-psod-gp-exception-13/
wordpress_id: 4583
categories:
- VMware
tags:
- '6.5'
- ESXi
- KB
- vSphere
topics: ["vSphere", "ESXi"]

---

While at a customer site, migrating an old vSphere 5.5 environment to 6.5, several hosts suddenly crashed with a PSOD during the migration. Long story short, we got hit by this: [VMware KB 2147958: _ESXi 6.5 host fails with PSOD: GP Exception 13 in multiple VMM world at VmAnon_AllocVmmPages (2147958_)](https://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2147958)

<!--more-->

**It turned out that a bunch of the VMs we were vMotioning from the old environment had the _cpuid.corePerSocket_ advanced setting set in the .vmx file, and this _can_ cause ESXi 6.5 to enter a state of panic, and in our case it certainly did.**

Upgrading the hosts to 6.5a, like the knowledgebase article states, alleviated the issue and we did not experience PSOD's again while migrating the 100+ VMs from the old environment to the new one.
