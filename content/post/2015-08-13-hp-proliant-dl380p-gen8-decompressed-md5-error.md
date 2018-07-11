---
author: shane
comments: true
date: 2015-08-13 01:35:44+00:00
layout: post
slug: hp-proliant-dl380p-gen8-decompressed-md5-error
title: HP Proliant DL380p Gen8 "Decompressed MD5" error
url: /virtualization/hp-proliant-dl380p-gen8-decompressed-md5-error/
wordpress_id: 3721
categories:
- Virtualization
---

![](http://vninja.net/wordpress/wp-content/uploads/gravatar/3898d54.jpg)This is a guest post from [Shane Williford](https://twitter.com/coolsport00)
Sr. Systems Engineer, VCAP-DCA/EMCCAe/Pizza Connoisseur and vExpert.





## Problem History



I work at a school district in the US (Kansas City area). After the school year ended, my Director decided he wanted to upgrade to vSphere6 from vSphere55U2 on a few Hosts we were using with XenApp. We are using XenApp to deliver apps to student labs that utilize an Autocad program. As such, our Hosts also have a graphics card in them – nVIDIA GRID K1. To give the students a bit more graphics power this upcoming school year, we added a 2nd nVIDIA card to each Host. The Hosts are HP Proliant DL380p Gen8 with Intel Xeon X5650 2.67GHz processors and about 296GB RAM. Since we added a 2nd nVIDIA card, we also needed to upgrade the Host power supplies to support the 2 cards’ power consumption (1200W support).

<!--more-->


In addition, we only had one 2-port Fiber Adapter in each Host. We wanted to add another for redundancy. The Adapter model used is **HP FlexFabric10Gb 2-port 554FLR-SFP Adapter**. Upon booting the Host after adding both the 2nd Adapter and the 2nd nVIDIA card, I experienced two issues:

Before the POST, a RSOD (red screen of death) displayed stating:  **NMI Detected. Consult Integrated Management log for details**

and then after I resolved that error, and attempted the vSphere6 install, I received the following:

[![Decompressed-MD5](/img/Decompressed-MD5-300x92.png)](/img//Decompressed-MD5.png)

When I reviewed the iLO log for the RSOD NMI error, I saw the following recurring messages:

[![iLO-Log](/img//iLO-Log-300x21.png)
](/img//iLO-Log.png)
After many attempts searching the “interwebz” for a resolution to both issues, and having no luck, it was time to ping HP Support. After several correspondences with HP and attempting different configs, below is what was done to finally resolve each issue.

I thought it would be worthwhile to share this with the community since 1. I found little to no posts on either issue in my resolution research, and 2. if anyone is using graphic cards to deliver visually-enhanced desktops as we are, this may prove beneficial and expedite any Host troubleshooting due to adding the aforementioned hardware.



## Resolution



**NMI Error**

  1. The 1st issue wasn’t too entirely difficult to figure out the cause, but was a bit cumbersome to figure out the resolution. The issue was caused by adding the 2nd Adapter in the 2nd riser card that also housed the 2nd nVIDIA card, as noted in the iLO log image shown above. To mitigate the NMI error, a BIOS setting needed changed:


  2. Enter the Setup Utility by pressing F9 during POST.[![setup-utility](/img/setup-utility.png)


  3. Arrow down to the 2nd option – Power Management Options, then press ENTER.


  4. Arrow down to Advanced Power Management Options, then press ENTER.[![Adv-Pwr-Mgmt](/img/Adv-Pwr-Mgmt-300x93.png)](/img/Adv-Pwr-Mgmt.png)


  5. Arrow down to Maximum PCI Express Speed and press ENTER to change the setting to:[![PCI-E-Speed-Setting](/img/PCI-E-Speed-Setting.png)](/img/PCI-E-Speed-Setting.png)


  6. Press ESC out of the Advanced Power Management Options window, ESC out of the Power Management Options winodw, then ESC again to exit out of the Setup Utility. When prompted, press F10 to Save and Exit the Utility and reboot the Host.



**Decompressed MD5:**





  1. After I resolved the NMI error, I attempted to install vSphere6. After the Host detected the vSphere package, I selected to install it and it began to unpack it. Towards the end of the unpack phase, the Decompressed MD5 error displayed. To resolve this error, I did the following:


  2. I upgraded my BIOS to the latest version. As of the date of this post (12 Aug 15), the BIOS firmware latest release is dated 2 Aug 2014 (A), meaning it was ‘amended’ (13 Oct 2014); see this link: [http://h20564.www2.hpe.com/hpsc/swd/public/detail?dwf.restartSession=true&sp4ts.oid=5194969&swItemId=MTX_e8d0275c17494b81b03e42bb3d&swEnvOid=4166#tab5](http://h20564.www2.hpe.com/hpsc/swd/public/detail?dwf.restartSession=true&sp4ts.oid=5194969&swItemId=MTX_e8d0275c17494b81b03e42bb3d&swEnvOid=4166#tab5)


  3. Press F9 to get into the main Setup Utility window (see Setup Utility image posted above).


  4. At the main Setup Utility window, press **CTL+A**, which will then add Service Options at the end of the list.[![setup-utility-SO](/img/setup-utility-SO-226x300.png)](/img/setup-utility-SO.png)


  5. Arrow down to Service Options and press ENTER; when in Service Options, arrow down to PCI Express 64-bit BAR Support.[![PCI-BAR-Suppt](/img/PCI-BAR-Suppt-300x178.png)](/img//PCI-BAR-Suppt.png)


  6. If this option is not set to Enabled, Press ENTER and set it to **ENABLED**.[![Enabled](/img/Enabled.png)](/img/Enabled.png)


  7. Press ESC out of the Service Options window, then ESC again to exit out of the Setup Utility. When prompted, press F10 to Save and Exit the Utility.


  8. After the Host reboots, re-attempt to Install ESXi. The Decompressed MD5 error should not display any longer.
