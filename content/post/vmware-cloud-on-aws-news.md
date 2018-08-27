---
type: "post"
date: "2018-08-26"
year: "2018"
author: Christian Mohn
topics: ["vSphere", "Virtualization", "VMware Cloud on AWS"]
tags: ["VMware", "VMworld 2018", "VMware Cloud on AWS"]

title: "VMware Cloud on AWS — New Amazon EC2 elastic, bare-metal instance for vSAN"

description: "VMware Cloud on AWS (vSAN) utilizing Amazon Elastic Block Storage (EBS) is an interesting one. Being able to independently increment storage in the VMC without adding compute nodes is a feature that has been missing, **until now**."

card: "summary_large_image"
site: "@vninjanet"
creator: "@h0bbel"
image: "https://vninja.net/logos/vmworld-2018-v1.jpg"
---

VMware vSAN utilizing Amazon Elastic Block Storage (EBS) is an interesting one. Being able to independently increment storage in the VMC without adding compute nodes is a feature that has been missing, **until now**.  

Customers of VMware Cloud on AWS will now be able to scale storage independently of compute, by adding new [EBS storage](https://aws.amazon.com/ebs/) nodes. 

![VMC on AWS - Storage Nodes](/img/vmconaws/vmconaws-vsan.png)

## Overview

### R5.Metal Physical Host Configuration

* ​3 disk groups per host
* All storage provided by EBS GP2
* Raw capacity tier of 15-35TB
    * Configured at Cluster creation
    * 5TB increments ​Compression Enabled

**Item** | **Available** |
---------|----------|
 CPU | Skylake-SP |
 Sockets per Host| 2 |
 Cores per Socket| 24 |
 Cores per Host| 48 |
 Threads per Host| 96 |
 Memory| 768 GB |
 Storage | EBS GP2 |
 NICs | 1 x ENA |

![VMC on AWS - Storage Nodes](/img/vmconaws/vmconaws-vsan2.png)

**Note:** This new storage cluster needs to be added to an existing SDDC and cannot be the first cluster that is provisioned in the customer environment.