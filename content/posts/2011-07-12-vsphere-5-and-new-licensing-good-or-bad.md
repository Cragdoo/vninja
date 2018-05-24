---
author: eczerwin
comments: true
date: 2011-07-12 18:36:07+00:00
layout: post
link: http://vninja.net/news/vsphere-5-and-new-licensing-good-or-bad/
slug: vsphere-5-and-new-licensing-good-or-bad
title: vSphere 5 and new licensing - Good or bad?
wordpress_id: 1266
categories:
- News
- Rant
- Virtualization
- VMware
tags:
- ESXi
- Licensing
- VMware
- vRam
- vSphere
- vSphere 5
---

As many of you did I watched todays Cloud Infrastructure Forum and the release of vSphere 5 today.  I was very excited with many of the features such as Storage Profiling, Storage DRS, VMFS 5 release, and they have blown the top off of the resource limits on VMs to create Monster VMs - just to mention a few.  However, one topic I notice causing quite a stir is the new licensing that seemed to be very briefly mentioned at the end of the webinar. To quote VMware in page 3 the [vSphere 5 licensing guide](http://www.vmware.com/files/pdf/vsphere_pricing.pdf):





<blockquote>

vSphere 5.0 will be licensed on a per-processor basis with a vRAM entitlement. Each vSphere 5.0 CPU license will entitle the purchaser to a specific amount of vRAM, or memory configured to virtual machines. The vRAM entitlement can be pooled across a vSphere environment to enable a true cloud or utility based IT consumption model. Just like VMware technology offers customers an evolutionary path from the traditional datacenter to cloud infrastructure, the vSphere 5.0 licensing model allows customers to evolve to a cloud-like “pay for consumption” model without disrupting established purchasing, deployment and license- management practices and processes.</blockquote>




This caused quite an uproar on twitter of people complaining that it would raise their licensing costs. My personal opinion on the new licensing is both negative and positive.  For every negative side I see in something I always try to put a positive spin on it.  Firstly it is true that this may cause some highly consolidated shops to have to reasses their infrastructure before they upgrade to vSphere 5.  It may require purchase of more licenses to obtain more pooled vRAM to be on the legal side of the licensing.  It may also slow adoption as people have to perform audits on their infrastructure to determine what will be needed for the new licensing model.  Also for some of the big memory packed beast servers this may prove to be a disadvantage. As I have heard thru the [vSphere 5 licensing guide](http://www.vmware.com/files/pdf/vsphere_pricing.pdf) there is no hard limit and vSphere will not stop you from deploying VMs for every licensing model but Essentials which there is actually a hard limit.

On a positive note; as a vSphere Admin this licensing may make my life easier.  When application owners realize that there is a charge based on memory use and they may need to sign a purchase order to get their oversized machine approved instead of making their application more efficient they may change their tune a bit.  This means less vm sprawl and more focus on what exactly is running in the environment and is it running at its absolute best and most efficient. Also  If there is a zombie VM comsuming some valuable vRAM I am sure it will also be found and dispatched more quickly than with the current licensing model.




<blockquote></blockquote>
