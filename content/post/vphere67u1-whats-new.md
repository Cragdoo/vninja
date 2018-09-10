---
type: "post"
date: "2018-08-26"
year: "2018"
author: Christian Mohn
topics: ["vSphere", "Virtualization", "VMware"]
tags: ["vSphere", "VMware", "VCSA", "VMworld 2018"]

title: "VMware vSphere 6.7u1 — What's New?"

description: "Another VMworld, another vSphere announcement. As all new releases, vSphere 6.7u1 comes with a bunch of new features and capabilities — **yay, finally feature complete HTML5 client!**"


card: "summary_large_image"
site: "@vninjanet"
creator: "@h0bbel"
image: "https://vninja.net/logos/vmworld-2018-v1.jpg"
---


# vSphere 6.7u1 Announced at VMworld 2018 US

Another VMworld, another vSphere announcement. vSphere 6.7u1 comes with a bunch of new features and capabilities, some of which are listed below:

### Feature Complete HTML5 Client

![vsphere 6.7u1 — HTML5 Client](/img/vsphere67u1/vsphere67u1-html5.png)

Finally the HTML5 client is feature complete! It's been a long wait, but in v6.7u1 it's here! With improved search, support for Auto Deploy, Host Profiles and vCenter High Availability (VCHA) this is your new home as a vSphere admin.

### vCenter Appliance Improvements

* **vCenter High Availability (VCHA)**
    * vSphere Client (HTML5) improved workflow
    * Auto detects when VCSA is being managed
    * REST APIs
    * Auto clone creation option for passive and witness nodes
    * Monitoring & Alerting
* **Built-in Firewall**
    * The vCenter Appliance now comes with a built-in firewall that is manages through VAMI.
* **Converge Tool**
    * This new tool enables moving of external PSC's (even load balanced ones) into an internal PSC, which is the preferred option going forward.
* **Improved Content Library**
    * Deploy from, Clone to, and Sync OVF and OVA templates
    * Guest customization
    * Support for native OVA and VMTX
    * Deploy from or Clone to
    * ISO mounting support
    * Store other files such as scripts
* **Cluster Quickstart**
    * The vSphere Cluster Quickstart is a new cluster, host and network configuration workflow. It enables easy addition of new or existing hosts, in bulk, with built-in pre-checks and recommendations. This complements the [vSAN Easy Install](https://storagehub.vmware.com/t/vmware-vsan/easy-install/) wizard, for end-to-end greenfield deployments.

    &nbsp;
    ![vSAN 6.7u1 — Cluster Quickstart](/img/vsan67u1/vsan67u1-quickstart.png)

 **Happy VMworld everyone, and [happy 20 Year Anniversary VMware](https://www.vmware.com/timeline.html)**