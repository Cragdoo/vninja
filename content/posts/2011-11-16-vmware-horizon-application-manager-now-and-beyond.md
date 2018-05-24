---
author: cmohn
comments: true
date: 2011-11-16 23:05:06+00:00
layout: post
link: http://vninja.net/virtualization/vmware-horizon-application-manager-now-and-beyond/
slug: vmware-horizon-application-manager-now-and-beyond
title: VMware Horizon Application Manager Now and Beyond
wordpress_id: 1612
categories:
- Virtualization
- VMware
tags:
- App delivery
- Horizon
- Horizon Application Manager
- SaaS
- ThinApp
- Virtual Application
- VMware
---

![](http://vninja.net/wordpress/wp-content/uploads/2011/11/VMware-Horizon-Application-Manager-1.21.png)VMware has announced [Horizon Application Manager 1.2](http://www.vmware.com/products/desktop_virtualization/horizon/), and together with the new [ThinApp 4.7](http://blogs.vmware.com/thinapp/2011/11/vmware-thinapp-47-whats-new.html) release it promises _"end users access to Windows, SaaS and enterprise web applications across different devices while retaining control and visibility via policy-driven management"_.



<blockquote>VMware Horizon Application Manager now manages your ThinApp applications making it easier and faster to provide virtualized Windows applications to end users. From Horizon Administration, you can deploy ThinApp packages, entitle users and groups, track user licenses, and manage application updates.</blockquote>



The coupling of the Horizon Application Manager with ThinApp is a great idea, and when I saw today's announcement I got pretty excited. The possibility to have your own internal application portal providing your end users with self-service installs of virtualized applications is great news and could potentially be really useful in a great number of organizations.

Sadly my initial excitement quickly faded when I realized that for now Horizon Application Manager is a hosted service, that requires an on premise connector in your infrastructure that sends over a limited set of Active Directory data to enable it to check user account or group access to the applications it offers. The connector provides single sign-on (Kerberos) functionality for users already authenticated in your Active Directory and authenticates the user to the Horizon service using SAML, so the hosted service never has the AD password. The hosted service does still needs some information like _samaccountname_, _first name_, _last name_, _email_ and a _GlobalUID_.

For more details, have a look at [Understanding VMware Horizon Application Manager](http://www.ntpro.nl/blog/archives/1911-Understanding-VMware-Horizon-Application-Manager.html) by [Eric Sloof](https://twitter.com/esloof).

This also means that users who run a virtualized application provisioned by Horizon Application Manager an active internet connection is required, even if the virtualized application packages are stored on a local (to the user) file share. Subsequent application launches does not require an active connection, as the applications are copied to the local system on the initial run. The Horizon agents retrieves a lease for the application, from the Horizon service, for an administrator configurable number of days (30 days default) and the end-user can run the application, without connecting to the Horizon service, until the lease expires or is renewed.

For many organizations, including mine, this poses a real problem. "Handing over" Active Directory data to a hosted service is not something I would want in my environment, especially when our use case would be to provide end users with a self-service application portal for local applications. Other organizations might look at that differently though, and this might not be a concern for all customers.

I understand that Horizon Application Manager was initially created for [SaaS](http://en.wikipedia.org/wiki/Software_as_a_service) scenarios where a hosted authentication portal makes sense. I also understand that this is the first version that provides integration with ThinApp, and this is very much a product still in development and refinement.

For now, Horizon Application Manager does not provide the use case that I was looking for but thankfully **[Ben Goodman](http://twitter.com/benontech)**, _Lead Evangelist for VMware Horizon_, has taken the time to address my call for an on-premise version of Horizon Application Manager:



<blockquote>I understand your apprehension. Horizon was built on top of technology originally designed exclusively to be a Single-Sign on service to SaaS applications. We are in the process of expanding that technology to become a true enterprise service. This is happening in two ways, the first is by adding application support beyond SaaS. The first step was Windows support via ThinApp and we are looking at other application platforms to follow. The second is evaluating options for moving some or all of product on-prem. Both of these steps are the primary focus of the development team over the next 12-18 months and we are really excited about where we are taking Horizon.</blockquote>



This is great news, an on-premise version that provides exactly what I'm looking for seems to be in the pipeline and on VMware's roadmap for Horizon Application Manager. I just wish I had it now, it would have been perfect for a project I'm working on at the moment that I hope to wrap up by the end of the year. 

Oh well, there is always next year and the next project!
