---
author: bajorgensen
comments: true
date: 2018-01-14 16:00:00+00:00
layout: post
slug: curious-case-intel-microcode-part-2-gets-better-worse
title: 'The Curious Case of the Intel Microcode Part #2 - It Gets Better — Then Worse'
url: /news/curious-case-intel-microcode-part-2-gets-better-worse/
wordpress_id: 4953
categories:
- News
tags:
- Analysis
- Intel
- Meltdown
- Security
- Spectre
- VMware
---

This guest post by [Bjørn Anders Jørgensen](https://twitter.com/bajorgensen), Senior Systems Consultant Basefarm, first appeared on [LinkedIn](https://www.linkedin.com/pulse/curious-case-intel-microcode-part-2-gets-better-worse-jørgensen/).

Before you start on this rather long post, [have a go at part #1](http://vninja.net/news/the-curious-case-of-the-intel-microcode/):

![](/img/spectre-text-252x300.png) ![](/img/meltdown-text-154x300.png)



##  tl;dr



This is a long read. To get to the juicy part on how Intel potentially shipped pre-release microcode to partners, skip to section 3. The short, short version is that the official Intel microcode update contains newer microcode for Skylake-SP and Kaby Lake/Coffe Lake than what currently is shipping from VMware/HPE/DellEMC etc.



## Section 1: The good



Last week I wrote about how Intel should improve their microcode update delivery mechanism and offer full disclosure on their microcode changes. Then this week events progressed in rapid succession:





  * Intel released updated microcode bundle 20180108


  * VMware released updated patches for vSphere, including microcode updates


  * Then it was discovered that the updates cause stability issues


  * Most computer vendors recalled BIOS updates for Haswell/Broadwell


  * VMware recommended not to expose VMs to the new CPU feature flag


  * I discovered that Xeon SP and Kaby Lake/Coffe Lake updates from most or all vendors is based on the pre-release Intel bundle 20171215!



First of all I have to say I feel for all the engineers and product managers taken by surprise when the news broke early on the Meltdown and Spectre vulnerabilities. Apparently the embargo was suppose to be lifted on the 9. January, and everyone was working towards this date. There must have been extreme pressure from peers and customers. Mistakes will be made, but could have been avoided if Intel was more transparent on their process and releases.

Lets start with the Intel microcode bundle. Deconstructing the bundle using MC extractor you'll find 94 binary files containing 196 individual microcode updates. Intel has released a single bundle with all updates going back to 1998. This is good! As far as I know there has not been a single update bundle going back this far. Additionally all vSphere patches contains microcode updates for most production systems running vSphere, including an update for AMD EPYC processors as described in the [security advisory](https://www.vmware.com/security/advisories/VMSA-2018-0004.html). You can find all the details on my [github repository](https://github.com/bajorgensen/Intel_microcode).

This is progress - however there are still no release notes or details except for the updated microcode versions for each processor family:



    - Updates upon 20171117 release --
    IVT C0          (06-3e-04:ed) 428->42a
    SKL-U/Y D0      (06-4e-03:c0) ba->c2
    BDW-U/Y E/F     (06-3d-04:c0) 25->28
    HSW-ULT Cx/Dx   (06-45-01:72) 20->21
    Crystalwell Cx  (06-46-01:32) 17->18
    BDW-H E/G       (06-47-01:22) 17->1b
    HSX-EX E0       (06-3f-04:80) 0f->10
    SKL-H/S R0      (06-5e-03:36) ba->c2
    HSW Cx/Dx       (06-3c-03:32) 22->23
    HSX C0          (06-3f-02:6f) 3a->3b
    BDX-DE V0/V1    (06-56-02:10) 0f->14
    BDX-DE V2       (06-56-03:10) 700000d->7000011
    KBL-U/Y H0      (06-8e-09:c0) 62->80
    KBL Y0 / CFL D0 (06-8e-0a:c0) 70->80
    KBL-H/S B0      (06-9e-09:2a) 5e->80
    CFL U0          (06-9e-0a:22) 70->80
    CFL B0          (06-9e-0b:02) 72->80
    SKX H0          (06-55-04:b7) 2000035->200003c
    GLK B0          (06-7a-01:01) 1e->22

    Note that 20171117 is Intels previous official release



Keep a mental note for version 80 for Kaby Lake/Coffee Lake and 200003c for Skylake-SP (Xeon SP). Also note the absence of Sandy Bridge updates.



## Section 2: The bad



With new microcode and OS patches available, tens if not hundreds of thousands of IT professionals spend much of last week planning and executing mitigation for the Meltdown and Spectre vulnerabilities. And with Spectre variant #2 (Branch Target Injection) requiring microcode update, I will assume most also started deploying updated BIOS and Intel patches through the OS mechanism for Linux and vSphere.

With the wider deployment it did not take long for customers to discover there where issues with system crashes or "higher system reboots" as [Intel marking tries to spin it](https://newsroom.intel.com/news/intel-security-issue-update-addressing-reboot-issues/). How are system administrators suppose to make a informed decision based on that? It is quite ironic that Intel CEO, Brian Krzanich at the same time makes his ["Security-First Pledge"](https://newsroom.intel.com/news-releases/security-first-pledge/) offering "Transparent and Timely Communications". It seems like Lenovo is the only vendor with [some actual details on the issues](https://www.google.no/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwid7J7gtNfYAhWMIJoKHSPFDV4QFggvMAE&url=https%3A%2F%2Fsupport.lenovo.com%2Fus%2Fen%2Fsolutions%2Flen-18282&usg=AOvVaw2SRw7Qjzz3sfucJBo0yxlS):





  1. (Kaby Lake U/Y, U23e, H/S/X) Symptom: Intermittent system hang during system sleep (S3) cycling. If you have already applied the firmware update and experience hangs during sleep/wake, please flash back to the previous BIOS/UEFI level, or disable sleep (S3) mode on your system; and then apply the improved update when it becomes available. If you have not already applied the update, please wait until the improved firmware level is available.


  2. (Broadwell E) Symptom: Intermittent blue screen during system restart. If you have already applied the update, Intel suggests continuing to use the firmware level until an improved one is available. If you have not applied the update, please wait until the improved firmware level is available.


  3. (Broadwell E, H, U/Y; Haswell standard, Core Extreme, ULT) Symptom: Intel has received reports of unexpected page faults, which they are currently investigating. Out of an abundance of caution, Intel requested Lenovo to stop distributing this firmware.



With all the system manufacturers removing their BIOS downloads for Haswell and Broadwell and [VMware recommending](https://kb.vmware.com/s/article/52345) to "hide the speculative-execution control mechanism for virtual machines" if patches have already applied, this cannot be described as nothing short of an industry wide recall. It is hard to say how bad it is, but if it is so bad that Intel is recalling the update and CEO and EVP posting publicly I think it is wise to await further updates and see how things pan out. Intel will release updated microcode soon and the various vendors will post new BIOS updates in the coming few weeks. Go to the VMware link and note the various processors affected in yellow, because it gets worse:



## Section 3: The ugly





<blockquote>Disclaimer: The following analysis is made with best effort on little information. Intel is shipping old and new versions in their microcode update bundle. It may be that they are for different CPU steppings of the same processor.</blockquote>





![](https://media.licdn.com/mpr/mpr/AAMAAQDGAAgAAQAAAAAAAA8QAAAAJDIyZmQxOGRhLTA1MGYtNDBjMC1iYWVmLTMxY2M5NmY5NjhkMg.jpg)



While all this is unfolding I am trying to piece together what the Intel and VMware patches actually contains. Using MC extractor I was able to inspect the microcode bundles, map the updates to CPU models and cross reference different releases. The complete list is on github. In the rush to get patches out, it seems like Intel made some last minute changes that was not communicated to partners. I have been in contact with several vendors and none seems to be aware that there changes made between the unofficial pre-release bundle from 15. December that was shared under embargo. Luckily this only affect three processor generations, but combined with the Haswell/Broadwell issues, it leaves us with an awful mess and a potential open vulnerability.



## To quote my one week younger self:





<blockquote>So with six months lead time Intel has not been able to release an official microcode bundle update. The latest official one is from 17. November and does not seem to contain any Spectre fixes. Worse, it seems like the unofficial bundle is only partially fixing variant #2/Spectre.</blockquote>



It is obvious that Intel made some last minute changes to microcode and failed to notify partners. What changes has been made between update bundle 20171215 and 20180108? Are we installing incomplete non-released microcode to millions of computers? Only Intel knows! It might be as simple as an effort to include some minor fixes now what everyone with an Intel processor have to update their computer, or it can be a monumental error where many computers are still partially vulnerable to Spectre even with patches installed. With the ["security first"](https://newsroom.intel.com/news-releases/security-first-pledge/) pledge from CEO Brian Krzanich promising transparency I expect and demand an explanation from Intel. How is it that VMware, HPE and DellEMC can ship pre-release microcode to millions of computers?



    Intel microcode update bundle pre-release 15. December
    +-------+--------------------------+---------+------------+---------+---------+----------+----------+--------+
    | CPUID |         Platform         | Version |    Date    | Release |   Size  | Checksum |  Offset  | Latest |
    | 50654 |  B7 [0, 1, 2, 4, 5, 7]   | 200003A | 2017-11-21 |   PRD   |  0x6C00 | C088D252 | 0x64400  |  Yes   |
    | 806E9 |        C0 [6, 7]         |    7C   | 2017-12-03 |   PRD   | 0x18000 | 5C75A5FE | 0x8B000  |   No   |
    | 806EA |        C0 [6, 7]         |    7C   | 2017-12-03 |   PRD   | 0x17C00 | B81BC926 | 0xA3000  |  Yes   |
    | 906E9 |       2A [1, 3, 5]       |    7C   | 2017-12-03 |   PRD   | 0x18000 | 6CF72404 | 0xBAC00  |   No   |
    | 906EA |        22 [1, 5]         |    7C   | 2017-12-03 |   PRD   | 0x17800 | 55695D1F | 0xD2C00  |  Yes   |
    | 906EB |          02 [1]          |    7C   | 2017-12-03 |   PRD   | 0x18000 | 5046D998 | 0xEA400  |  Yes   |
    +-------+--------------------------+---------+------------+---------+---------+----------+----------+--------+

    Intel microcode update bundle released 8. january
    +-------+--------------------------+---------+------------+---------+---------+----------+----------+--------+
    | CPUID |         Platform         | Version |    Date    | Release |   Size  | Checksum |  Offset  | Latest |
    +-------+--------------------------+---------+------------+---------+---------+----------+----------+--------+
    | 50654 | B7 [0, 1, 2, 4, 5, 7]    | 200003C | 2017-12-08 |   PRD   | 0x6C00  | A4059069 |  0x0     |  Yes   |
    | 806E9 | C0 [6, 7]                |    80   | 2018-01-04 |   PRD   | 0x18000 | 6961A256 |  0x0     |  Yes   |
    | 806EA | C0 [6, 7]                |    80   | 2018-01-04 |   PRD   | 0x18000 | F6263DAE |  0x0     |  Yes   |
    | 906E9 | 2A [1, 3, 5]             |    80   | 2018-01-04 |   PRD   | 0x18000 | 6AA1DE93 |  0x0     |  Yes   |
    | 906EA | 22 [1, 5]                |    80   | 2018-01-04 |   PRD   | 0x17C00 | 84CABC68 |  0x0     |  Yes   |
    | 906EB |  02 [1]                  |    80   | 2018-01-04 |   PRD   | 0x18000 | D24EDB7F |  0x0     |  Yes   |
    +-------+--------------------------+---------+------------+---------+---------+----------+----------+--------+



Note that all updates are labeled PRD for production. If we take the [VMware matrix ](https://kb.vmware.com/s/article/52345)and mark the recalled microcode in yellow and the potential pre-release in red, we get a pretty depressing chart:



![](https://media.licdn.com/mpr/mpr/AAMAAQDGAAgAAQAAAAAAAA0XAAAAJGEyNGU5MjIyLWJlNGUtNDM1MC05MzZkLWRmYTc0YzBkODMwNg.png)



The same versions was also shipped in Dell BIOS updates. Thanks to Dell engineering for actually listing the microcode updates, I have not seen the same transparency from other vendors. HPE take note!



    R740/R740xd/R640/R940/7920R

    Updated the Intel Xeon Processor Scalable Family Processor Microcode to version 0x3A.

    R830

    Updated the Xeon Processor E5-2600 v3 Product Family Processor Microcode to version 0x3B
    Updated the Intel Xeon Processor E5-2600 v4 Product Family Processor Microcode to version 0x0b000025

    R630/R730/R730XD

    Updated the Xeon Processor E5-2600 v3 Product Family Processor Microcode to version 0x3B
    Updated the Intel Xeon Processor E5-2600 v4 Product Family Processor Microcode to version 0x0b000025

    R930

    Updated the Intel Xeon processor E7-4800/8800 v3 Product Family Processor Microcode to version 0x10
    Updated the Intel Xeon processor E7-4800/8800 v4 Product Family Processor Microcode to version 0x0b000025



The Rx30 updates have now been removed from the Dell web page due to the aforementioned issues, but the R740 update is still there. Should it be recalled as well? Hard to tell, I think an update from Intel is in order.



![](https://media.licdn.com/mpr/mpr/AAMAAQDGAAgAAQAAAAAAAAxHAAAAJDU1OTQyNzNmLWNlYjctNDIzZi05MzM0LTkzZDNlZWVjZmRlOA.png)



I have not been able to attempt to load the new microcode on a server to confirm that it actually would apply, but I tested on my Precision 5520 laptop that use a Xeon E3-1505M v6 processor with the help of the [VMware microcode update driver for Windows](https://labs.vmware.com/flings/vmware-cpu-microcode-update-driver) and the microcode will update from 7C to 80:



## Some advice



So what is a system administrator to do? My advice is to wait it out. The situation is still fluid and changes almost on a daily basis. Let the dust settle and hope for some more clarity in the coming week. Remember that microcode updates are only for Spectre variant #2 vulnerability which is the hardest to exploit. If you already started patching vSphere, consider a rollback or implement the workaround from VMware.

Focus on patching Meltdown on soft targets such as laptops. These devices are much more likely to be running untrusted code. Dont forget applications, especially browsers. Researchers has been able to exploit meltdown and read non-cached data. The hackers can not be far behind. Combining Meltdown with a remote exploit could be disastrous.

https://twitter.com/aionescu/status/952014225714511872








As everything [except Rasberry Pi](https://www.raspberrypi.org/blog/why-raspberry-pi-isnt-vulnerable-to-spectre-or-meltdown/) is vulnerable to Spectre, focus on mobiles and tablets. Map you vulnerable devices and vendor updates. Pace yourself, we will be fighting this for years to come. There will be more side-channel vulnerabilities in the future and this is not the last round of patching.

Keep in mind that there are no microcode updates for Sandy Bridge processors yet. This is why Dell and HPE is holding back updates for Poweredge G12 servers and Proliant Gen8 servers. If you have a lot of Xeon 4600/2600/1600/1400/1200 V1 servers, you will have to wait on Intel and server vendors if you want full mitigation for Spectre.
