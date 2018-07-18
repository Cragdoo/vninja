---
author: Christian Mohn
comments: true
date: 2013-04-26 12:36:07+00:00
layout: post
slug: quick-dirty-esxi-5-1u1-upgrade
title: Quick and Dirty ESXi 5.1U1 Upgrade
url: /vmware-2/quick-dirty-esxi-5-1u1-upgrade/
wordpress_id: 2559
categories:
- VMware
tags:
- Command Line
- ESXi
- Host
- Upgrade
- VMware
---

Now that [VMware ESXi 5.1 Update 1](http://www.vmware.com/support/vsphere5/doc/vsphere-esxi-51u1-release-notes.html) has been released I decided to do a quick and dirty upgrade of my home installation. I refuse to call it a lab these days, since it´s one singular host and all it does it contain my home domain controller...

Anyway, the following procedure upgraded the host from 5.1b to 5.1U1, by downloading the upgrade directly from VMware and installing it. **Make sure the host is in maintenance mode before attempting this procedure.**

[Check Correlating vCenter Server and ESXi/ESX host build numbers to update levels (1014508)](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1014508) to determine which of the update files in the repository to download.

SSH into your host, and issue the following command:

{{< highlight bash >}}
~ # esxcli software profile update -d https://hostupdate.vmware.com/software/VUM/PRODUCTION/main/vmw-depot-index.xml -p ESXi-5.1.0-20130402001-standard
{{< /highlight >}}

This will initiate a download of the new ESXi version and install the update automatically, beware that it will not show any progress bar or indication while the file is being downloaded from the VMware repository.

The command above will only work, as [Erik](http://twitter.com/ErikBussink) pointed out in the [comments](http://vninja.net/vmware-2/quick-dirty-esxi-5-1u1-upgrade/#comment-12275), if you have allowed the httpClient outgoing access on the ESXi server. If not, you can enable it by running the following command on the host (or by using the vSphere Client):
{{< highlight bash >}}
~ # esxcli network firewall ruleset set -e true -r httpClient
{{< /highlight >}}

You can of course monitor your download with the vSphere Client (web or otherwise), to make sure nothing has stopped.

[![QuickDirtyUpgrade01](/img/QuickDirtyUpgrade011-300x83.png)](/img/QuickDirtyUpgrade011.png)

You can also monitor the upgrade process by looking at the VMware logs [Monitoring the ESXi Upgrade Process](http://vninja.net/vmware-2/monitoring-esxi-upgrade-process/) as Paul suggested in the [comments](http://vninja.net/vmware-2/quick-dirty-esxi-5-1u1-upgrade/#comment-12609).

Once it´s done, it will look like this:

{{< highlight bash >}}

Update Result
Message: The update completed successfully, but the system needs to be rebooted for the changes to be effective.
Reboot Required: true
VIBs Installed: VMware_bootbank_esx-base_5.1.0-1.12.1065491, VMware_bootbank_esx-xserver_5.1.0-0.11.1063671, VMware_bootbank_ipmi-ipmi-si-drv_39.1-4vmw.510.1.12.1065491, VMware_bootbank_misc-drivers_5.1.0-1.12.1065491, VMware_bootbank_net-bnx2_2.0.15g.v50.11-7vmw.510.1.12.1065491, VMware_bootbank_net-bnx2x_1.61.15.v50.3-1vmw.510.0.11.1063671, VMware_bootbank_net-e1000e_1.1.2-3vmw.510.1.12.1065491, VMware_bootbank_net-igb_2.1.11.1-3vmw.510.1.12.1065491, VMware_bootbank_net-ixgbe_3.7.13.6iov-10vmw.510.1.12.1065491, VMware_bootbank_net-tg3_3.123b.v50.1-1vmw.510.1.12.1065491, VMware_bootbank_scsi-megaraid-sas_5.34-4vmw.510.1.12.1065491, VMware_locker_tools-light_5.1.0-1.12.1065491
VIBs Removed: VMware_bootbank_esx-base_5.1.0-0.10.1021289, VMware_bootbank_esx-xserver_5.1.0-0.0.799733, VMware_bootbank_ipmi-ipmi-si-drv_39.1-4vmw.510.0.0.799733, VMware_bootbank_misc-drivers_5.1.0-0.0.799733, VMware_bootbank_net-bnx2_2.0.15g.v50.11-7vmw.510.0.0.799733, VMware_bootbank_net-bnx2x_1.61.15.v50.3-1vmw.510.0.0.799733, VMware_bootbank_net-e1000e_1.1.2-3vmw.510.0.0.799733, VMware_bootbank_net-igb_2.1.11.1-3vmw.510.0.0.799733, VMware_bootbank_net-ixgbe_3.7.13.6iov-10vmw.510.0.0.799733, VMware_bootbank_net-tg3_3.110h.v50.4-4vmw.510.0.0.799733, VMware_bootbank_scsi-megaraid-sas_5.34-4vmw.510.0.0.799733, VMware_locker_tools-light_5.1.0-0.9.914609
VIBs Skipped: VMware_bootbank_ata-pata-amd_0.3.10-3vmw.510.0.0.799733, VMware_bootbank_ata-pata-atiixp_0.4.6-4vmw.510.0.0.799733, VMware_bootbank_ata-pata-cmd64x_0.2.5-3vmw.510.0.0.799733, VMware_bootbank_ata-pata-hpt3x2n_0.3.4-3vmw.510.0.0.799733, VMware_bootbank_ata-pata-pdc2027x_1.0-3vmw.510.0.0.799733, VMware_bootbank_ata-pata-serverworks_0.4.3-3vmw.510.0.0.799733, VMware_bootbank_ata-pata-sil680_0.4.8-3vmw.510.0.0.799733, VMware_bootbank_ata-pata-via_0.3.3-2vmw.510.0.0.799733, VMware_bootbank_block-cciss_3.6.14-10vmw.510.0.0.799733, VMware_bootbank_ehci-ehci-hcd_1.0-3vmw.510.0.0.799733, VMware_bootbank_esx-dvfilter-generic-fastpath_5.1.0-0.0.799733, VMware_bootbank_esx-tboot_5.1.0-0.0.799733, VMware_bootbank_esx-xlibs_5.1.0-0.0.799733, VMware_bootbank_ima-qla4xxx_2.01.31-1vmw.510.0.0.799733, VMware_bootbank_ipmi-ipmi-devintf_39.1-4vmw.510.0.0.799733, VMware_bootbank_ipmi-ipmi-msghandler_39.1-4vmw.510.0.0.799733, VMware_bootbank_misc-cnic-register_1.1-1vmw.510.0.0.799733, VMware_bootbank_net-be2net_4.1.255.11-1vmw.510.0.0.799733, VMware_bootbank_net-cnic_1.10.2j.v50.7-3vmw.510.0.0.799733, VMware_bootbank_net-e1000_8.0.3.1-2vmw.510.0.0.799733, VMware_bootbank_net-enic_1.4.2.15a-1vmw.510.0.0.799733, VMware_bootbank_net-forcedeth_0.61-2vmw.510.0.0.799733, VMware_bootbank_net-nx-nic_4.0.558-3vmw.510.0.0.799733, VMware_bootbank_net-r8168_8.013.00-3vmw.510.0.0.799733, VMware_bootbank_net-r8169_6.011.00-2vmw.510.0.0.799733, VMware_bootbank_net-s2io_2.1.4.13427-3vmw.510.0.0.799733, VMware_bootbank_net-sky2_1.20-2vmw.510.0.0.799733, VMware_bootbank_net-vmxnet3_1.1.3.0-3vmw.510.0.0.799733, VMware_bootbank_ohci-usb-ohci_1.0-3vmw.510.0.0.799733, VMware_bootbank_sata-ahci_3.0-13vmw.510.0.0.799733, VMware_bootbank_sata-ata-piix_2.12-6vmw.510.0.0.799733, VMware_bootbank_sata-sata-nv_3.5-4vmw.510.0.0.799733, VMware_bootbank_sata-sata-promise_2.12-3vmw.510.0.0.799733, VMware_bootbank_sata-sata-sil24_1.1-1vmw.510.0.0.799733, VMware_bootbank_sata-sata-sil_2.3-4vmw.510.0.0.799733, VMware_bootbank_sata-sata-svw_2.3-3vmw.510.0.0.799733, VMware_bootbank_scsi-aacraid_1.1.5.1-9vmw.510.0.0.799733, VMware_bootbank_scsi-adp94xx_1.0.8.12-6vmw.510.0.0.799733, VMware_bootbank_scsi-aic79xx_3.1-5vmw.510.0.0.799733, VMware_bootbank_scsi-bnx2i_1.9.1d.v50.1-5vmw.510.0.0.799733, VMware_bootbank_scsi-fnic_1.5.0.3-1vmw.510.0.0.799733, VMware_bootbank_scsi-hpsa_5.0.0-21vmw.510.0.0.799733, VMware_bootbank_scsi-ips_7.12.05-4vmw.510.0.0.799733, VMware_bootbank_scsi-lpfc820_8.2.3.1-127vmw.510.0.0.799733, VMware_bootbank_scsi-megaraid-mbox_2.20.5.1-6vmw.510.0.0.799733, VMware_bootbank_scsi-megaraid2_2.00.4-9vmw.510.0.0.799733, VMware_bootbank_scsi-mpt2sas_10.00.00.00-5vmw.510.0.0.799733, VMware_bootbank_scsi-mptsas_4.23.01.00-6vmw.510.0.0.799733, VMware_bootbank_scsi-mptspi_4.23.01.00-6vmw.510.0.0.799733, VMware_bootbank_scsi-qla2xxx_902.k1.1-9vmw.510.0.0.799733, VMware_bootbank_scsi-qla4xxx_5.01.03.2-4vmw.510.0.0.799733, VMware_bootbank_scsi-rste_2.0.2.0088-1vmw.510.0.0.799733, VMware_bootbank_uhci-usb-uhci_1.0-3vmw.510.0.0.799733
~ #{{< /highlight >}}

Reboot your host, and behold the glory of the new ESXi 5.1 Update 1 version!

[![QuickDirtyUpgrade02](/img/QuickDirtyUpgrade022-300x65.png)](/img/QuickDirtyUpgrade022.png)

If you want to know how to figure out how to find the correct URL and file, check out William Lam´s excellent [A Pretty Cool Method of Upgrading to ESXi 5.1](http://www.virtuallyghetto.com/2012/09/a-pretty-cool-method-of-upgrading-to.html) post, which provides more details.

_I told you it was quick and dirty, didn`t I?
_
