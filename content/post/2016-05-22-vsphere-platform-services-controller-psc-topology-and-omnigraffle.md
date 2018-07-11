---
author: Christian Mohn
comments: true
date: 2016-05-22 19:19:52+00:00
layout: post
slug: vsphere-platform-services-controller-psc-topology-and-omnigraffle
title: vSphere Platform Services Controller (PSC) topology and Omnigraffle
url: /virtualization/vsphere-platform-services-controller-psc-topology-and-omnigraffle/
wordpress_id: 4100
categories:
- Virtualization
tags:
- automation
- Omnigraffle
- PSC
- Script
- Topology
- vSphere
---

A little while ago [William Lam](https://twitter.com/lamw) published a little python script called [extract_vsphere_deployment_topology.py](http://www.virtuallyghetto.com/2016/05/generating-vcenter-server-platform-services-controller-deployment-topology-diagrams.html) that basically lets you export your current vSphere PSC topology as a [DOT (graph description language)](https://en.wikipedia.org/wiki/DOT_(graph_description_language)) file. Great stuff, and in itself useful as is, especially if you run it through [webgraphviz.com](http://www.webgraphviz.com) as William suggests.

The thing is, you might want to edit the topology map, change colours and fonts, and even move the boxes around, after you get the output. If you have a large environment, you might want to combine all your PSC topologies into a single document? It turns out, that's pretty easy to do!
<!--more-->

[Omnigraffle Pro](https://www.omnigroup.com/omnigraffle) imports the DOT files natively, and lets you play around with the objects as if they were drawn in Omnigraffle from the beginning. Save the output from the script somewhere as a .dot file. Then open Omnigraffle and go to _File -> Open_ and select the file.![Screenshot 2016-05-22 21.10.12](/img/Screenshot-2016-05-22-21.10.12-243x300.png)

Now, select the _Hierarchical_ option, and you'll get a nicely formatted canvas with your PSC components already laid out inside of Omnigraffle. Now you can edit it at will!

![Screenshot 2016-05-22 21.13.56](/img/Screenshot-2016-05-22-21.13.56-1024x708.png)

As far as I can tell, this isn't possible with Microsoft Visio, as it doesn't support the DOT format, but you could always save it as a Visio file with Omnigraffle if you need to sent it to your more Microsoft inclined friends.

I'm sure there are more fun to be had with these DOT files, it's just text files after all, perhaps someone can even code up a script that converts them to Visio .vdx files or some other format that Visio can import natively.
