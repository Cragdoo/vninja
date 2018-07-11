---
author: Christian Mohn
comments: true
date: 2016-02-24 10:42:26+00:00
layout: post
slug: running-a-vsan-poc-customer-reactions
title: Running a VSAN PoC - Customer reactions
url: /virtualization/running-a-vsan-poc-customer-reactions/
wordpress_id: 4038
categories:
- Virtualization
tags:
- Proof-of-Concept
- storage
- VSAN
---

I recently set up a VMware Virtual SAN 6.1 Proof-of-Concept for a customer, configuring a 3-node cluster based on the following setup:
<!--more-->


### Hardware:![c04411605](/img/c04411605-300x225.png)

  * HP ProLiant DL380 G9

  * 2 x Intel Xeon E5-2680 @ 2.50Ghz w/12 Cores

  * 392 GB RAM

  * 1 x Intel DC 3700 800GB NVMe

  * 6 x Intel DC S3610 1.4TB SSD

  * HP FlexFabric 556FLR-SFP+ 10GBe NICs


### Virtual SAN Setup:

Since this was a simple PoC setup, the VSAN was configured with 1 disk group pr host with all 6 Intel DC S3610 drives used as the capacity layer, and the [Intel DC P3700 NVMe](http://www.intel.com/content/www/us/en/solid-state-drives/intel-ssd-dc-family-for-pcie.html) cards set up as the cache. This gives a total of **21.61TB** of usable space for VSAN across the cluster. With the _Failures-To-Tolerate=1_ (the only real FTT policy available in a three node 6.1 cluster) policy this gives 10.8TB of usable space.

vMotion and VSAN traffic were set up to run on a separate VLANs over 2 x 10GBe interfaces, connected to a Cisco backend.



### Customer reaction:



After the customer had been running it in test for a couple of weeks, I got a single line email from them simply stating: "**WOW!**".

They were so impressed with the performance (Those NVMe cards are FAST!) and manageability of the setup that they have now decided to order an additional 3 hosts, bringing the cluster up to a more reasonable 6 hosts, in a metro-cluster setup, and upgrade to [VSAN 6.2](https://www.vmware.com/products/whats-new-virtual-san) as soon as it's available.  The compression, deduplication and erasure coding features of 6.2 will increase their available capacity just by upgrading. At the same time, adding three new hosts will effectively double the available physical disk space as well, even before the 6.2 improvements kick in.

VSAN will be this customers preferred storage platform going forward and they can finally move off they existing monolithic, and expensive, FC SAN over to a storage solution that outperforms it and greatly reduces complexity.
