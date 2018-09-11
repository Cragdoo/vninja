---
author: Christian Mohn
comments: true
date: 2016-11-22 13:05:33+00:00
layout: post
slug: cohesity-my-initial-impression
title: 'Cohesity: My Initial Impression'
url: /virtualization/cohesity-my-initial-impression/
wordpress_id: 4401
categories:
- Virtualization
tags:
- Backup
- Cohesity
- storage
topics: ["Review", "Backup", "Cohesity"]

---

![cohesity-logo](/img/Cohesity-logo-300x232.png)

A few weeks back [Cohesity](http://cohesity.com) gave me access to a lab environment, where I could play around with their HyperConverged Secondary Data solution. For those unaware of their offering entails, it's simply put a solution for managing secondary storage. In this case, secondary storage is really everything that isn't mission critical. It can be your backups, test/dev workloads, file shares and so on . The idea to place these unstructured data sets on a secondary storage platform, to ease management and analytics but at the same time keep it integrated with the rest of the existing environment. It's a Distributed scale-out platform, with a pay-as-you-grow model.

Currently Cohesity supports both SMB and NFS as data entry points, and it also supports acting as a front-end for Google Cloud Storage Nearline, Microsoft Azure, Amazon S3 and Glacier.

<!--more-->


### Partial Feature List



I won't go through a complete feature list for the current v3.0 offering, but here are a few highlights:





  * Replication between Cohesity Clusters


  * Physical Windows and Linux support (in addition to VMs)


  * Single object restore for MS SQL, MS Sharepoint and MS Exchange.


  * Archival of data to Azure, Amazon, Google


  * Tape support


  * Data Analytics



Getting data out of your VMs, and onto a secondary storage tier makes sense, even more so when you can replicate that data out of your datacenter as well. This makes your VMs smaller and thus easier to manage.

Naturally I was most interested in looking at this from a vSphere perspective, and that's what I had a look at in the lab. Backups and Clones are presented back to the vSphere environment using NFS, something that enables quick restore and cloning without massive data transfers to get started.

_Without any introduction to the product what so-ever I was able to create Protection Jobs (backups) and clone VMs directly from the Cohesity interface._



## [![screenshot-2016-11-05-23-04-26](/img/Screenshot-2016-11-05-23.04.26-300x216.png)](/img/Screenshot-2016-11-05-23.04.26.png)Creating Protection Jobs:



Creating a Proection Job is easy, select the VMs you want to protect from the infrastructure:[
](/img/Screenshot-2016-11-05-23.05.23.png) [![screenshot-2016-11-05-23-06-37](/img/Screenshot-2016-11-05-23.06.37-300x210.png)](/img/Screenshot-2016-11-05-23.06.37.png)

Select, or create a Protection Policy (did I mention it's policy driven?)

![screenshot-2016-11-05-23-07-15](/img/Screenshot-2016-11-05-23.07.15-300x172.png)

Watch the backups run

[![screenshot-2016-11-06-23-44-25](/img/Screenshot-2016-11-06-23.44.25-300x220.png)](/img/Screenshot-2016-11-06-23.44.25.png)[![screenshot-2016-11-06-23-49-24](/img/Screenshot-2016-11-06-23.49.24-300x193.png)](/img/Screenshot-2016-11-06-23.49.24.png)



## Creating Clones



The procedure for clone jobs is equally simple

[![screenshot-2016-11-06-23-45-32](/img/Screenshot-2016-11-06-23.45.32-300x269.png)](/img/Screenshot-2016-11-06-23.45.32.png) [![screenshot-2016-11-06-23-46-23](/img/Screenshot-2016-11-06-23.46.23-300x214.png)](/img/Screenshot-2016-11-06-23.46.23.png)




The Cohesity 3.0 UI is beautiful and easy to work with. As I mentioned in my tweet after looking at this for a little under an hour:


{{< tweet 795029165833650176 >}}


It'll be interesting to see where this moves from here, but from a purely technical point of view the current offering looks pretty darn good! Of course, I've only scratched the surface here playing with backup/restore and cloning only, the platform has much more to offer besides that.

