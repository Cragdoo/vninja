---
author: eczerwin
comments: true
date: 2011-07-17 19:31:22+00:00
layout: post
link: http://vninja.net/virtualization/smb-shared-storage-smackdown-part-1-nfs-performance/
slug: smb-shared-storage-smackdown-part-1-nfs-performance
title: SMB Shared Storage Smackdown - Part 1 NFS Performance
wordpress_id: 1310
categories:
- Virtualization
---

Recently at the office I was given the task to test out some SMB NAS products to use as potential candidates for some of our small branch offices all over the world.  I did many tests relating from backup and replication to actually running VMs on them and pounding them with IOmeter.  What I will share with you in this series of  posts is my vSphere/IOmeter tests for NFS and iSCSI. With these tests my main goal was to see what kinds of IO loads the NAS devices could handle and also how suitable they would be for running a small vSphere environment.  In my next post I will go into iSCSI performance and will publish all of my results including NFS into a downloadable PDF.

**NAS Devices**



	
  * Synology DS411+

	
  * NetGear ReadyNAS NV+

	
  * QNAP TS4139 Pro II+

	
  * Iomega StorCenter ix4-200d


I chose all 4 drive models of the arrays and they are all filled with 1 TB SATA disks.  It was chosen this way since we would also be using these devices to hold rather large backups and replicate them elsewhere.

To start out with I upgraded all of them to the latest firmware and created RAID 5 arrays on each of them.  To make a long story short this gave me anywhere from 2.5 TB - 2.8 TB usable storage on each device.  Since I tested both NFS and iSCSI I first created a 1 TB iSCSI Lun (1MB block size on Datastore) on each device then created an NFS export for the remainder of the space.  Another small note is I was sure that write cacheing was enabled for all arrays that had an option to have to have turned on or off.  Then I got down to setting up vSphere and the rest of my hardware.

**Server/VM/Network Infrastructure**



	
  * 1 HP DL380 G5 - 2 quad core pCPUs - 16 logical cores with HT enabled 16GB of pRAM - ESXi 4.1 U1 installed default configuration

	
  * Win2k8 VMs on each NAS Device 24GB boot device VMDK 100GB VMDK with standard LSI logic SAS connector 1 vCPU and 4096 vRAM

	
  * DELL Powerconnect 5524 Switch - Split up into VLAN for VMs/vSphere Management and VLAN for iSCSI/NFS traffic


I began the task of plugging everything in, getting everything set up properly as not to skew any results, and spinning up VMs in the Datastores from the attached iSCSI Luns/NFS Exports.  It is important to note that for each Shared Storage Datastore I created a new VM in the exact specifications as above via Template and  aligned all disks to 64k. For the connection to the storage I only had 1 extra 1gbe Nic per server.  Then in ESXi I created a seperate standard vSwitch just for iSCSI/NFS traffic.  If you are interested in the setup of my lab infrastructure please [Contact me](http://www.twitter.com/eczerwin) and I will be happy to go more indepth.



**Testing Method and Results Collection**

After playing around with Iometer for some time and searching around to find a standard I decided to use the tests from the very popular VMware communities [Open unofficial storage performance thread](http://communities.vmware.com/thread/73745).  The exact ICF file I used can also be found there for download if you would like to do some of your own tests.  Regardless of the age of some of the posts I still think this is the most relevent and fair measure possible.  These tests include



	
  * Max Throughput-100% Read

	
  * RealLife-60% random - 65% Read

	
  * Max Throughput-50% Read

	
  * Random-8k 70% Read


First important thing to mention is only the VM generating the load was powered on during each test so there should be no skewing based on this. I put the IOmeter files into a set of Excel bar graphs.  I decided to base my results on what I call the big 3; Total IOps, MBps, and Average latency.

**NFS Final Performance Results**

_**NFS Max Throughput 100% Read**_

[![](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS100ReadIOps.jpg)](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS100ReadIOps.jpg)

[![](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS100ReadMBps.jpg)](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS100ReadMBps.jpg)

[![](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS100ReadART.jpg)](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS100ReadART.jpg)



_** RealLife-60% random - 65% Read**_

[![](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS60RandomIOps1.jpg)](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS60RandomIOps1.jpg)
[![](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS60RandMBps.jpg)](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS60RandMBps.jpg)

[![](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS60RandART.jpg)](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS60RandART.jpg)





_**Max Throughput-50% Read**_

[![](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS50ReadIOps.jpg)](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS50ReadIOps.jpg)

[![](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS50ReadMBps.jpg)](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS50ReadMBps.jpg)

[![](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS50ReadART.jpg)](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS50ReadART.jpg)





_**Random-8k 70% Read**_

[![](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS70RandIOps.jpg)](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS70RandIOps.jpg)

[![](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS70RandMBps.jpg)](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS70RandMBps.jpg)

[![](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS70RandART.jpg)](http://vninja.net/wordpress/wp-content/uploads/2011/07/NFS70RandART.jpg)



**Final Conclusions For NFS and closing**

As you may have noticed by the assorted graphs the two winners to come out in the NFS performance tests were QNAP and Synology.  QNAP appears to be slightly better at more Random workloads such as a real life vSphere environment and Synology seems to be way ahead of most of the arrays with solid sequential storage access - which would be perfect for a backup device.  However, they all seem to have high Average Response Times during Random workloads which in my opinon makes or breaks how well an environment runs.  From this first look I would say most of these NAS devices would be just fine for shared storage in a very small lab environment, a possible backup target, or for something simple as a fileserver volume.  In my next post we can put it all together with the iSCSI results and declare the final winner of the **SMB Shared Storage Smackdown**!!!


