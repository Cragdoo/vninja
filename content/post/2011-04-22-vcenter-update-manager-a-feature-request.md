---
author: cmohn
comments: true
date: 2011-04-22 21:07:35+00:00
layout: post
slug: vcenter-update-manager-a-feature-request
title: vCenter Update Manager - A Feature Request
url: /virtualization/vcenter-update-manager-a-feature-request/
wordpress_id: 1079
categories:
- Virtualization
tags:
- ESX
- ESXi
- Ops
- vCenter
- vCenter Update Manager
- VMware
---

Way back in August 24 2010 I wrote a post called [vCenter Update Manager to lose it’s fat](http://vninja.net/virtualization/vcenter-update-manager-to-lose-its-fat/). I'm still very happy that VMware has decided to drop OS patching from the product, and I still mean that can only be a good thing. In fact, that article prompted [Beth Pariseau](http://twitter.com/#!/PariseauTT) Senior News Writer for [searchservervirtualization.techtarget.com](http://searchservervirtualization.techtarget.com) to call me when researching her [VMware users eye changes to Update Manager](http://searchservervirtualization.techtarget.com/news/2240035040/VMware-users-eye-changes-to-Update-Manager)  article. 

I would like to expand a bit on the following quote from the article:


<blockquote>
Centralized management is fine, Mohn said, but he’d like to see satellite servers hang on to a local repository of patches, which can then be applied with a command from the central server ...
</blockquote>



As I work with small ROBO environments, which in many cases have low bandwidth available coupled with very high latency, using VMware Update Manager to update the sites is often not feasible. The sheer problem with installing the patches from a central HQ based repository, and the time it consumes and potential failure rates, makes it more practical to download the patches manually to a [local vMA installation](http://vninja.net/virtualization/using-vma-as-a-local-vsphere-patch-repository/) (possibly even [replicated with rsync](http://vninja.net/virtualization/using-rsync-to-distribute-patches-to-remote-a-vma/)) and applying the patches to the host from a local repository. This also minimizes the hosts downtime when applying patches.

**What I would like to see added to VMware Update Manager is the ability to tell a remote host to apply patches, but from a local file repository**. 

Using vMA for this is absolutely possible, but I can also see using local (to the host) NAS storage as the patch repository as another possibility. By using some DNS magic, it would even be possible to tell all remote vSphere hosts to fetch their updates from _\\patchrepo\vmware\_ (or something similar) and it would still be a local repository. 

VMware Update Manager could even handle the replication of the patches to the remote sites, but in general I'm in favor of using the existing underlying network infrastructure that's already in place to move the patches from a central location to the remote locations.

So, in short, all I'm asking is for a way to tell a central VMware Update Manager installation where the patches for a remote server is, and invoke the patching process from the central installation. Surely, that can't be too much to ask for, can it?
