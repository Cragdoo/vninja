---
author: Christian Mohn
comments: true
date: 2012-12-04 22:49:27+00:00
layout: post
slug: vmware-vsphere-health-checks-wha
title: VMware vSphere Health Check - What´s in it for You?
url: /vmware-2/vmware-vsphere-health-checks-wha/
wordpress_id: 2242
categories:
- VMware
tags:
- Best Practice
- ESXi
- Health Check
- realworld
- vCenter
- VMware
- vSphere
---



[VMware vSphere Health Check](http://www.vmware.com/files/pdf/services/consserv-vmware-vsphere-health-check-datasheet.pdf) is a service provided by VMware Professional Services, either by their own consultants or by a consultant from a VMware Certified Solution Provider.

I´m lucky enough to be employed by a Premier VMware Solution Provider who offers this service to our customers, and I´ve completed a fair number of these health checks in the last couple of months.

You might ask what the value of the Health Check is, and for the end customer it´s pretty clear cut. They get a very detailed report on the current status of their vSphere infrastructure, complete with action items and concise recommendations for improvements. Of course, I sell these service professionally and therefore I´m biased by default.

_What I want to focus on here though, is not the value customers get out of the Health Check, but rather the incredible learning opportunity this service provides for **you** as a consultant._

The data gathering part of the Health Check is done via software developed by VMware. The software is provided in two flavors, either as a ThinApped Windows Application or as a virtual appliance that you deploy in the customers environment. Pick your poison, but I´ve mostly worked with the ThinApp version. It gathers a lot of interesting data from the target vCenter, and you can then take that data with you and generate a report based on that.

That´s where the fun starts. The HealthAnalyzer software comes with a web interface where you can look at everything it collected and prioritize the different findings. Of course, it also comes with suggested priorities from VMware, for the given data. This is where the learning is for everyone conducting the health checks.

By reading, and understanding the automated observations generated through the data collection you have a wealth of information available, complete with "best practices" recommendations from VMware (These are updated quarterly). "Best practices" is in itself a loaded term, and you should not blindly follow them without knowing what actually makes a best practice just that. It might not be applicable in the environment you are analyzing, but the thought process involved with looking at the data and determining if this is applicable has a lot of value in and of itself. Of course, like any good consultant the real answer is always "it depends..."


### Examples:
[![](/img/HealthCheck1-300x160.png)](/img/HealthCheck1.png)


It´s hard to come up with arguments against the situation in the screenshot, but this is just an example of how detailed the information provided is.

Some observations even come with links to relevant documentation providing more background on why something is recommended:

[![](/img/HealthCheck2-300x146.png)](/img/HealthCheck2.png)

For me, this has been an invaluable resource for learning and understanding why VMware recommends certain practices.

If you work for a service provider, do yourself a big favor and check if you have access to the HealthAnalyzer software on VMware Partner Central and if you do, download and play around with it.

_Even if you don´t plan on doing these checks as a service for new or existing customers, running it after you have done an install is a great, and automated, way of checking if your installation adheres to the standards it should. And who knows, chances are that you´ll end up learning a lot in the process. There is just no way that can be wrong._




