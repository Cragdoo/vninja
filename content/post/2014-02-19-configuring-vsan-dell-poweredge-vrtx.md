---
author: Christian Mohn
comments: true
date: 2014-02-19 12:30:08+00:00
layout: post
slug: configuring-vsan-dell-poweredge-vrtx
title: Configuring VSAN on a Dell PowerEdge VRTX
url: /virtualization/configuring-vsan-dell-poweredge-vrtx/
wordpress_id: 3078
categories:
- Virtualization
tags:
- Dell
- ESXi
- Hardware
- storage
- VMware
- VRTX
- VSAN
---

![OriginalJPG](/img/OriginalJPG.jpeg)

The [Dell PowerEdge VRTX shared infrastructure platform](http://www.dell.com/us/business/p/poweredge-vrtx/pd) is interesting, and I've been lucky enough to be able to borrow one from Dell for testing purposes.

One of the things I wanted to test, was if it was possible to run [VMware VSAN](http://www.vmware.com/products/virtual-san) on it, even if the Shared PERC8 RAID controller it comes with is not on the [VMware VSAN HCL](http://www.vmware.com/resources/compatibility/search.php), nor does it provide a method to do passthrough to present raw disks directly to the hosts.

<!--more-->


My test setup consists of:





  * 1 Dell PowerEdge VRTX


    * SPERC 8


      * 7 x 300GB 15k SAS drives





    * 2 x Dell PowerEdge M520 blades


      * 2 x 6 core Intel Xeon E5-2420 @ 1.90Ghz CPU


      * 32 GB RAM


      * 2 x 146GB 15 SAS drives









Both M520 blades were installed with  with ESXi 5.5, which is not a supported configuration from Dell. Dell has only certified ESXi 5.1 for use on the VRTX, but 5.5 seems to work just fine, with one caveat: **Drivers for the SPERC8 controller is not included in the Dell customized image for ESXi 5.5. To get access to the volumes presented by the controller,  the [6.801.52.00 megaraid-sas driver](http://www.vmware.com/resources/compatibility/detail.php?deviceCategory=io&productid=34775&deviceCategory=io&keyword=shared&page=1&display_interval=10&sortColumn=Partner&sortOrder=Asc) needs to be installed after ESXi 5.5. **

Once that is installed, the volumes will appear as storage devices on the host.

Sadly the SPERC8 controller does not support passthrough for disksin the PowerEdge VRTX chassis, something VSAN wants (For details check [VSAN and Storage Controllers](https://blogs.vmware.com/vsphere/2013/09/vsan-and-storage-controllers.html)). For testing purposes though, there is a way around it.

By creating several RAID0 Virtual Volumes on the controller, each one with only one disk in it, and assigning these disks to dedicated hosts in the chassis it is possible to present the disks to ESXi in a manner that VSAN can work with:



[![Dell PowerEdge VRTX VSAN Virtual Disks](/img/CMC-FLBNZY1-Manage-Virtual-Disks-2014-02-18-13-14-41-2014-02-18-13-14-43-1024x489.png)](/img/CMC-FLBNZY1-Manage-Virtual-Disks-2014-02-18-13-14-41-2014-02-18-13-14-43.png)



A total of six RAID0 volumes have been created, three for each host.



[![Dell PowerEdge VRTX Disk Assignment](/img/CMC-FLBNZY1-Assign-Virtual-Disks-2014-02-18-13-15-45-2014-02-18-13-15-47-1024x489.png)](/img/CMC-FLBNZY1-Assign-Virtual-Disks-2014-02-18-13-15-45-2014-02-18-13-15-47.png)



Each host gets granted exclusive access to three disks, resulting in them being presented as storage devices in vCenter



[![RawDisksVRTX](/img/RawDisksVRTX.png)](/img/RawDisksVRTX.png)



Since I don't have any SSD drives in the chassis, something that is a requirement of VSAN, I also had to fool ESXi into believing one of the drives was in fact SSD. This is done by changing the claim rule for the given device. Find the device ID in the vSphere Client, and run the following commands to mark it as an SSD:

(Check [KB2013188 Enabling the SSD option on SSD based disks/LUNs that are not detected as SSD](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2013188) by default for details.)

{{< highlight bash >}}
~ # esxcli storage nmp satp rule add --satp VMW_SATP_LOCAL --device naa.6b8ca3a0edc7a9001a961838899ee72a --option=enable_ssd
~ # esxcli storage core claiming reclaim -d naa.6b8ca3a0edc7a9001a961838899ee72a
{{< /highlight >}}

Once that part is taken care of, the rest of the setup is done by following the VSAN setup guides found in the [beta community](http://www.vsanbeta.com/).



[![VSAN Configured](/img/New-Document-Royal-TSX-2014-02-19-12-27-47-2014-02-19-12-27-50.png)](/img/New-Document-Royal-TSX-2014-02-19-12-27-47-2014-02-19-12-27-50.png)



Two Dell PowerEdge M520 nodes up and running, with VSAN, replicating between them inside a Dell PowerEdge VRTX chassis. Pretty nifty!

_It is worth noting is that in this setup, the SPERC8 is a single point of failure, as it provides disk access to all of the nodes in the same cluster. This is not something you want to have in a production environment, but Dell does offer a second SPERC8 option for redundancy purposes in the PowerEdge VRTX._

I did not do any performance testing on this setup, mostly since I don't have SSD's available for it, nor does it make much sense to do that kind of testing on a total of 6 HDD spindles;
**This is more a proof of concept setup than a production environment.**
