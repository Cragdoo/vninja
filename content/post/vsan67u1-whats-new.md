---
type: "post"
date: "2018-08-26"
year: "2018"
author: Christian Mohn
topics: ["Storage", "vSAN", "VMware"]
tags: ["vSAN", "VMware", "VMworld 2018"]

title: "VMware vSAN 6.7u1 — What's New?"

description: "VMworld 2018 US is upon is, and as per usual this means a lot of new announcements. One of them is vSAN 6.7u1 which comes with a bunch of new and useful features. This release mainly focuses on improved operations and maintenance, with a bunch of nice new additions."

twitter:
card: "summary_large_image"
site: "@vninjanet"
creator: "@h0bbel"
twimage: "https://vninja.net/logos/vmworld-2018-v2.jpg"
---

[VMworld 2018 US](https://www.vmworld.com/en/us/about.html) is upon us, and as per usual this means a lot of new announcements. 
One of them is vSAN 6.7u1 which comes with a bunch of new and useful features. This release mainly focuses on improved operations and maintenance, with a bunch of nice new additions. 

I've focused on a couple of them here, but this is not a defintive list!

### Simplified Upgrade Experience Through VMware Update Manager (VUM)

Firmware and drivers for vSAN has now been moved into VMware Update Manager (VUM) for centralized management, this means that the vSAN IO Controller Firmware Tool now integrated into VUM. 
#### Features

* ​Retrieves and updates firmware for the vSAN Cluster
* Supports custom ISOs for OEM specific builds
* Supports Dell HBA330 updates
* Supports vCenter without internet connectivity
* Per-host updating with additional safeguards
* Included in system-managed baselines


![vSAN 6.7u1 — vSAN Update Manager 2](/img/vsan67u1/vsan67u1-updatemanager2.png)


### Cluster Quickstart
The vSphere Cluster Quickstart is a new cluster, host and network configuration workflow. It enables easy addition of new or existing hosts, in bulk, with built-in pre-checks and recommendations. 

It complements the [vSAN Easy Install](https://storagehub.vmware.com/t/vmware-vsan/easy-install/) wizard, for end-to-end greenfield deployments.

![vSAN 6.7u1 — Cluster Quickstart](/img/vsan67u1/vsan67u1-quickstart.png)


### Improved Space Efficiency Using Storage Reclamation

Yes! Finally we get space reclamation from guests on vSAN as well. 

#### TRIM/UNMAP integration Features

* Reclaimed guest OS capacity
* Reclaims storage for vSAN
* Reduces use of destager
* Supports guest OS initiated TRIM/UNMAP commands
    * Windows Server 2012 or Windows 8 and newer
    *   Linux supporting ext4, xfs, btrfs, etc.
* Automated using guest OS online mode
* Scheduled mode supported

![vSAN 6.7u1 — TRIM/UNMAP](/img/vsan67u1/vsan67u1-trimunmap.png)

### Improved Resource Decommissioning Safeguards

Enter Maintenance Mode’ (EMM) operational improvements which includes a ​EMM “fail fast” pre-check simulation which determines success/failure with no data movement

* ​New warnings for: 
    * Hosts already in maintenance mode
    * Ongoing resyncs
    * Multipledecommission actions
    * Object repair timer adjustment available in UI


### Other items

* Improved vSAN Cluster Capacity insight, with historical capacity, deduplication and compression reports 
A new usable capacity estimator, whoch takes the selected storage policy into account and easy estimation of “what if” when using space efficient RAID-5/6

* Enhancements to vRealize Operations within vCenter, where dashboards are now capable of displaying vSAN stretched cluster intelligence with full Interoperability with vRealize Operations v7.0

* New PowerCLI 10.2 cmdlets that replace 18 Ruby vSphere Console (RVC) commands. Greater visibility into Cluster configuration info, Resyncing status, Healthchecks object info and status as well as  vSAN disk stats

There are other improvements as well, but I'll leave those to a later post — Preferrably once I get my hands on the actual vSAN 6.7u1 bits!

 **Happy VMworld everyone, and [happy 20 Year Anniversary VMware](https://www.vmware.com/timeline.html)**