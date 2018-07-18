---
author: Christian Mohn
comments: true
date: 2011-01-18 08:10:02+00:00
layout: post
slug: powershell-scripting-best-practices-analyzer-remotely
title: 'Powershell: Scripting Best Practices Analyzer remotely'
url: /windows/powershell-scripting-best-practices-analyzer-remotely/
wordpress_id: 800
categories:
- Windows
tags:
- Ops
- Powershell
- Remoting
---

![](/img/PowerShell.png)[Jan Egil Ring](http://twitter.com/#!/janegilring) over at [blog.powershell.no](http://blog.powershell.no) has created a great PowerShell script that lets you run the [Microsoft Best Practices Analyzer](http://technet.microsoft.com/en-us/library/dd392255%28WS.10%29.aspx) on remote Windows Server 2008 R2 machines. 

In short, [Invoke-BPAModeling.ps1](http://blog.powershell.no/2010/08/17/invoke-best-practices-analyzer-on-remote-servers-using-powershell)  queries your Active Directory for any machines that run Windows Server 2008 R2, runs BPA on them (if Windows PowerShell Remoting is enabled) and emails you the report. 

Great tool that should be in every Windows Server admins tool-belt, and probably also set as a scheduled job to make sure you stay up to date on your servers status.

