---
author: Christian Mohn
comments: true
date: 2016-11-23 13:57:56+00:00
layout: post
slug: vmware-vsan-2016-edition
title: 'VMware vSAN: 2016 Edition'
url: /virtualization/vmware-vsan-2016-edition/
wordpress_id: 4408
categories:
- Virtualization
tags:
- storage
- VMware
- VSAN
---

Both in 2014 and in 2015 I wrote pieces on the current status of VMware vSAN, and it's time to revisit it for 2016.

My previous posts:

2014: [VSAN: The Unspoken Future](http://vninja.net/virtualization/vsan-unspoken-future/)
2015: [VMware VSAN: More than meets the eye.](http://vninja.net/virtualization/vmware-vsan-more-than-meets-the-eye/)

vSAN 6.5 was released with vSphere 6.5, and brings a few new features to the table:

<!--more-->





  * **Virtual SAN iSCSI Target Service**


  * **Support for Cloud Native Apps running on the Photon Platform**


  * **REST APIs and Enhanced PowerCLI support**


  * **2-Node Direct Connect**


    * Witness Traffic Separation for ROBO


  * **All-Flash support in the standard license (Deduplication and compression still needs advanced or enterprise)**


  * **512e drive support**



In my opinion, the first three items on that list are the most interesting. Back in [2015](http://vninja.net/virtualization/vmware-vsan-more-than-meets-the-eye/) I talked about VMware turning vSAN into a generic storage platform, and the new Virtual SAN iSCSI Target Service is a step in that direction. This new feature allows you to share vSAN storage directly to physical servers over iSCSI (VMware is not positioning this as a replacement for ordinary iSCSI SAN arrays), without having to do that through iSCSI targets running inside a VM.

The same goes for Cloud Native Apps support, where new applications can talk with vSAN directly through the API, [even without a vCenter](http://cormachogan.com/2016/11/23/photon-controller-v1-1-vsan-interoperability/)!

Both of these bypass the VM layer entirely, and provide external connectivity into the core vSAN storage layer.

**Clearly these are the first steps towards opening up vSAN for external usage, expect to see more interfaces being exposed externally in future releases.** An object store resembling [Amazon S3](http://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html) doesn't sound too far fetched, does it? Perhaps even with back-end replication and archiving built-in. Stick your files in a vSAN and let the policies there determine which object should be stored locally, and which should be stored on S3? Or which should be replicated to another vSAN cluster, located somewhere else?

Being able to use [SBPM](http://blogs.vmware.com/vsphere/2014/09/storage-policy-based-management-overview.html) for more than VM objects is a good idea, and it makes management of those non-VM workloads running in your vSAN cluster easier to monitor and manage.

Sure the rest of the items on the list are nice too, the two 2-node Direct Connect feature allows you to connect two nodes without the need for external 10 GbE switches, cutting costs in those scenarios. All-Flash support on all license levels is also nice, but as is the case with [512e](https://en.wikipedia.org/wiki/Advanced_Format#512e) drive support, it's natural progression. With the current price point on flash devices, the vSAN Hybrid model is not going to get used much going forward.

_All in all, the vSAN 6.5 release is a natural evolution of a storage product that's still in it's infancy. That's the beauty of SDS, new features like this can be made available without having to wait for a hardware refresh._
