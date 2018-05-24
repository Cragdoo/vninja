---
author: cmohn
comments: true
date: 2011-04-20 07:01:37+00:00
layout: post
slug: installing-and-running-vmware-compliance-checker-for-vsphere
title: Installing and running VMware Compliance Checker for vSphere
url: /news/installing-and-running-vmware-compliance-checker-for-vsphere/
wordpress_id: 1055
categories:
- Howto
- News
- Virtualization
tags:
- Ops
- Security
- Software
- VMware
- Windows
---

The first version of the new VMware Compliance Checker for vSphere tool is now [available for download](https://www.vmware.com/tryvmware/?p=compliance-checker&lp=1). 

VMware Compliance Checker for vSphere lets you scan your ESX and ESXi hosts for compliance with the [VMware vSphere hardening guidelines](http://communities.vmware.com/docs/DOC-15413) to make sure your hosts are properly configured. It also lets you save and print your assessment results, so you can track your compliance level over time, or use them as documentation for internal audits.



### Installing VMware Compliance Checker for vSphere


After downloading the _VMwareComplianceCheckerForvSphere.msi_ installing is done in a matter of seconds, using the all to familiar _click Next to continue_ Windows installation routine. The tool is Windows only at this point.


[![](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-1-300x234.png)](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-1.png)
[![](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-2-300x234.png)](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-2.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-3-300x234.png)](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-3.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-4-300x234.png)](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-4.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-5-300x234.png)](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-5.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-6-300x234.png)](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-6.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-7-300x234.png)](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-7.png)

The tool is Java based, so the client machine you run it on needs to have it installed locally before you can use it.



### Running a Compliance Scan


Running a compliance scan is very easy. Start up VMware Compliance Checker for vSphere and point it towards either a ESX/ESXi host, or towards your vCenter installation.

[![](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-91-300x173.png)](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-91.png)

[![](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-10-300x176.png)](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-10.png)

The tool runs for a while, and in the end you'll be presented with a nice HTML based report highlighting all your compliance shortcomings!

[![](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-11-300x209.png)](http://vninja.net/wordpress/wp-content/uploads/2011/04/Installing-and-running-VMware-Compliance-Checker-for-vSphere-11.png)



### Impressions/Conclusion


VMware Compliance Checker for vSphere looks like it can be a valuable tool to add to your vAdmin tool-belt. In it's first version it does a good job of identifying potential issues with your environment. As far as I can see, [William Lam's](http://twitter.com/#!/lamw) Perl based [vSphere Security Hardening Report Script](http://www.virtuallyghetto.com/2011/01/updated-vsphere-security-hardening.html) does more extensive checks for now.

The vSphere Security Hardening Report Script also has a couple of other advantages, one being that it's operating system agnostic (since it's Perl based) another advantage is that since it's written in a scripting language you can set up automated cron jobs that performs the scanning for you. As far as I can see the VMware tool is missing the ability to schedule scans, which is something I really hope VMware will add to it in the not to distant future.

