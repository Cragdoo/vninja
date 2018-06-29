---
author: cmohn
comments: true
date: 2018-01-29 22:49:53+00:00
layout: post
slug: bootbank-cannot-be-found-at-path
title: Alerting on "Bootbank cannot be found at path ‘/bootbank’" in vRealize Operations
url: /vmware-2/bootbank-cannot-be-found-at-path/
wordpress_id: 4963
categories:
- VMware
tags:
- Alerting
- vRealize Log Insight
- vRealize Operations
- vRLI
- vROps
- vSphere
---

If you boot your ESXi hosts from SD-cards or USB you might have run into this issue. Suddenly your host(s) displays the following under events:
**"Bootbank cannot be found at path ‘/bootbank’."**

Usually this means that the boot device has been corrupted somehow, either due to a device failure or [other issues](https://kb.vmware.com/s/article/2144283). Normally the host continues to run, until it's rebooted that is...

<!--more-->

For some reason, vRealize Operations doesn't pick this up as a host issue that it alerts on, so if your alerting regime is based on vROps alerts, you might not get alerted immediately. Thankfully there is a way to remedy this, **and** have vROps and vRealize Log Insight work together at the same time.

On order for this to work, you need to have configured the [vRealize Log Insight Integration with vRealize Operations](https://docs.vmware.com/en/VMware-Validated-Design/4.1/com.vmware.vvd.sddc-deploya.doc/GUID-3DFD518E-E5E5-4B10-B4B4-65BFE0BA13A0.html) first.

Log in to Log Insight and search for **"Bootbank cannot be found at path ‘/bootbank’.". **If you want to restrict it even more, use two filters. One for **vc_event_type = exists**, and one for the text search itself.

![](/img/Screenshot-2018-01-29-23.07.40-300x169.png)Click on the little red bell icon and select _"Create Alert from Query"_.

This will bring up the "Edit Alert" window, where you can define your information.

![](/img/Screenshot-2018-01-29-23.07.19-644x794.png)Create a proper Description and Recommendation for the alert, and enable _"Send to vRealize Operations Manager"_. You also need to specify a Fallback Option. The Fallback Option is basically which object Log Insight should attach the alert to, if the originating object isn't found in vRealize Operations.

And that's it really, as long as the vLI and vROps integration is configured and working, it's easy add your own custom alerts in vRLI, and have them pop up in vROps.

If you want to copy my Description and Recommendations, here they are:



#### Description:



_"The device containing the VMware ESXi bootbank can not be found. This may be because of a boot device failure. Specific details should be available in the symptom details. For more information, check the Tasks & Events pane for the host in the vSphere Web Client"_



#### Recommendation:



_"Change or replace the boot device, if necessary. Contact the hardware vendor for assistance. After the problem is resolved, the alert will be canceled when the sensor that reported the problem indicates that the problem no longer exists."_


