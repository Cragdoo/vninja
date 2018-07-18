---
author: Christian Mohn
comments: true
date: 2012-10-29 21:48:08+00:00
layout: post
slug: hp-bl460c-g8-lose-its-datastore
title: Why Did the HP BL460c G8 Lose It's Datastore?
url: /vmware-2/hp-bl460c-g8-lose-its-datastore/
wordpress_id: 2166
categories:
- VMware
tags:
- BL460c
- c3000
- c7000
- ESXi
- G8
- HP
- P220i
- Patch
- ProLiant
- Update
- VMware
---


Post Update:
As per [Preetam´s](http://vcp5.wordpress.com) [comment](http://vninja.net/vmware-2/hp-bl460c-g8-lose-its-datastore/#comment-12163), HP has published a [customer advisory regarding the usage of install vs update](http://h20000.www2.hp.com/bizsupport/TechSupport/Document.jsp?objectID=c03603069&jumpid=em_alerts_us-us_Dec12_xbu_all_all_1990986_137282_proliantserversbladesystem_critical_006_0).



![HP BL 460c G8](/img/IMG_3814-2-300x225.jpg)

After installing a couple of brand new **HP ProLiant BL460c G8** blades with **HP Smart Array P220i** controllers at a customer site, I decided that I should upgrade from the [VMware-ESXi-5.0.0-Update1-623860-HP-5.20.43.iso](https://my.vmware.com/web/vmware/details?downloadGroup=HP-ESXI-5.0.0-U1-15MAR2012&productId=229) Build _623860_ used to install the blades, to the latest _821926_ [build offered by VMware](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2032584).

Normally this is a really easy process using VMware Update Manager, but since this is a new installation all the prerequisites for that are not in place just yet and I decided to use _esxcli_ to perform the update.

After placing the downloaded _ESXi500-201209001.zip_ VIB file on the local datastore on the first host, I ran the update with the following command: (abbreviated for legibility)

[cc lang="bash" width="95%" theme="blackboard" nowrap="0"]
~ # esxcli software vib install -d /vmfs/volumes/datastore1/ESXi500-201209001.zip
Installation Result
Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
Reboot Required: true
VIBs Installed: [...]
VIBs Removed: [...]
~ #
[/cc]

So far so good! One quick, as quick as a HP blade can, reboot later my host was updated to _ESXi 5.0.0 Build 821926_ and everything looked fine. That is, until I wanted to put some new files into the blades local storage. Much to my surprise, there was no local storage to be seen at all. Thinking that something must have gone wrong while booting, I decided to try a new restart and imagine my surprise when the blade booted up again, but this time with _Build 623860_.

Thinking that I must have messed things up pretty badly, I decided to try the same procedure on the second HP ProLiant BL460c G8 blade installed in the same manner. And yet again, I got the same results.

While this is somewhat comforting when contemplating Albert Einstein´s [definition of insanity](http://www.brainyquote.com/quotes/quotes/a/alberteins133991.html), it´s not comforting when it comes to the procedure I followed for updating the hosts.

This time around I recorded the output the _esxcli_ command gave me, and found this little gem hidden inside the output:(unrelated VIBs removed)

[cc lang="bash" width="95%" theme="blackboard" nowrap="0"]
~ # esxcli software vib install -d /vmfs/volumes/esx5local/ESXi500-201209001.zip
Installation Result
Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
Reboot Required: true
VIBs Installed: [...], VMware_bootbank_scsi-hpsa_5.0.0-17vmw.500.0.0.469512, [...]
VIBs Removed: [...], Hewlett-Packard_bootbank_scsi-hpsa_5.0.0-28OEM.500.0.0.472560, [...]
~ #
[/cc]

So, in short the update from VMware removes the HP Bootbank VIB for local SCSI controllers, and replaces it with a VMware one. Effectively this removes the ESXi hosts ability to read the local datastore from the **HP Smart Array P220i Controller**.

**What still baffles me though, is that the first boot after applying the update boots _Build 821926_, but subsequent boots are on _Build 623860_...**

In the end, to rectify the issue, I found the [scsi-hpsa-5.0.0-28OEM.500.0.0.472560.x86_64.vib](http://h20565.www2.hp.com/portal/site/hpsc/template.PAGE/public/psi/swdDetails/?sp4ts.oid=5177950&spf_p.tpst=swdMain&spf_p.prp_swdMain=wsrp-navigationalState%3Didx%253D%257CswItem%253DMTX_c169c54cc80d4c1c8515b79d21%257CswEnvOID%253D4115%257CitemLocale%253D%257CswLang%253D%257Cmode%253D%257Caction%253DdriverDocument&javax.portlet.begCacheTok=com.vignette.cachetoken&javax.portlet.endCacheTok=com.vignette.cachetoken) file on hp.com, downloaded it and placed it on the host.

I then ran the update again
[cc lang="bash" width="95%" theme="blackboard" nowrap="0"]
~ # esxcli software vib install -d /vmfs/volumes/datastore1/ESXi500-201209001.zip
Installation Result
Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
Reboot Required: true
VIBs Installed: [...]
~ #
[/cc]

After installing the update, I then installed the **HP ProLiant Smart Array Controller Driver**
{{< highlight bash >}}
~ # esxcli software vib install -v file:/tmp/scsi-hpsa-5.0.0-28OEM.500.0.0.472560.x86_64.vib
Installation Result
Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
Reboot Required: true
VIBs Installed: Hewlett-Packard_bootbank_scsi-hpsa_5.0.0-28OEM.500.0.0.472560
VIBs Removed: VMware_bootbank_scsi-hpsa_5.0.0-17vmw.500.0.0.469512
VIBs Skipped:
~ #
{{< /highlight >}}

This reverses the removal of the _Hewlett-Packard_bootbank_scsi-hpsa_5.0.0-28OEM.500.0.0.472560_ VIB by the VMware Update itself.

Another (quick) reboot, and finally my host was upgraded to the correct build and the local datastore was available too.

{{< highlight bash >}}
~ # cat /proc/driver/hpsa/hpsa0
hpsa0: HP Smart Array P220i Controller
Board ID: 0x3355103c
Firmware Version: 3.04
Driver Version: HP HPSA Driver (v 5.0.0-28OEM)
Driver Build: 2
IRQ: 217
Logical drives: 1
Current Q depth: 0
Current # commands on controller: 0
Max Q depth since init: 0
Max # commands on controller since init: 4
Max SG entries since init: 129
Max Commands supported: 1020
SCSI host number: 0

hpsa0/C0:B0:T0:L1 Direct-Access LOGICAL VOLUME 3.04 RAID 1(1+0)
~ #
{{< /highlight >}}

Thankfully these blades had not yet been put into production, and I was free to wrestle with them as much as I wanted to make this work, without it affecting anything other than my troubleshooting genes.

I have not tested if the same scenario unfolds if the hosts are updated via VMware Update Manager, but I suspect that the results would have been the same.

Of course, the hosts could have been installed with the newer [HP ESXi 5.0 U1 Sept 2012 refresh](https://my.vmware.com/web/vmware/details?downloadGroup=HP-ESXI-5.0.0-U1-15MAR2012_V2&productId=229) in the first place, and I probably would not have run into this issue, at least not until someone decided to apply a later patch to the host.

All in all, I guess the lesson here is that you need to be careful when updating your hosts and make sure you have a real retreat option ready if you need to quickly roll back to the previous build. The luxury of having the time and possibility to really troubleshoot the issue, might not be available to you if you are upgrading production systems.

_Test your updates, every time._
