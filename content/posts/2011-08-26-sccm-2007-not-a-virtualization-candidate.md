---
author: cmohn
comments: true
date: 2011-08-26 08:10:54+00:00
layout: post
link: http://vninja.net/microsoft-2/sccm-2007-not-a-virtualization-candidate/
slug: sccm-2007-not-a-virtualization-candidate
title: SCCM 2007 Not a Virtualization Candidate?
wordpress_id: 1399
categories:
- Microsoft
tags:
- Microsoft
- Ops
- realworld
- SCCM
- Windows
---

[![](http://vninja.net/wordpress/wp-content/uploads/2011/08/6451b-150x150.jpg)](http://vninja.net/wordpress/wp-content/uploads/2011/08/6451b.jpg)

The last couple of days I've been in training class, taking the [6451B Planning, Deploying and Managing Microsoft System Center Configuration Manager 2007](http://www.microsoft.com/learning/en/us/Course.aspx?ID=6451B) course.

One of the first things that got mentioned, was that for larger deployments you should not run System Center Configuration Manager virtually. Of course, this caught my eye as I'm a proponent for the virtualize first "movement".

It runs out that the reason for this is that Configuration Manager is somewhat poorly designed, as just about everything it receives from the clients in the network is placed in [text based log files (inbox folder)](http://systemcentercentral.com/BlogDetails/tabid/143/IndexID/80792/Default.aspx) before being processed and pumped into the back-end SQL DB. SCCM lives for and eats [log files](http://technet.microsoft.com/en-us/library/bb892790.aspx) for a living.

I'm sure there are good reasons for this, especially back in the day when SCCM 2007 was developed, but in retrospect this seems to be a poor design choice. Especially since the IO intensity of writing text-based log files is high, and doesn't scale well when you have loads of clients which SCCM 2007 supposedly is designed for in the first place.

There are ways of alleviating the strain on the machine running SCCM, like running the SQL server on a different server and running the management console on your local computer (remember a Windows server is tuned, by default, to prioritize background tasks), but the fact remains that sum of all the small write operations SCCM constantly does to your storage puts a heavy strain on it.

**So, if you want to run SCCM 2007 virtualized in your environment, make sure your storage is up to the task and that you don't saturate it by deploying what is in essence management software. Perhaps it is better to run it on a physical server with adequate local storage, but don't blame it on virtualization, blame it on poor design in SCCM 2007.**

Hopefully [Configuration Manager 201](http://www.microsoft.com/systemcenter/en/us/configuration-manager/cm-vnext-beta.aspx)2, which is currently in beta 2, behaves better when it's released. If not, how will Microsoft defend not getting real performance when running it in Hyper-V (or any other Hypervisor).
