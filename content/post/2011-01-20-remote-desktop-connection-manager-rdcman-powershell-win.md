---
author: Christian Mohn
comments: true
date: 2011-01-20 14:51:46+00:00
layout: post
slug: remote-desktop-connection-manager-rdcman-powershell-win
title: Remote Desktop Connection Manager (RDCMan) + Powershell = Win
url: /howto/remote-desktop-connection-manager-rdcman-powershell-win/
wordpress_id: 806
categories:
- Howto
tags:
- Ops
- Powershell
- RDCMan
- Windows
---

[Remote Desktop Connection Manager](http://msexchangeteam.com/archive/2010/06/11/455115.aspx) is a great tool from Microsoft which enables you to keep track of all your RDP sessions and targets in a nice GUI. One of the things it's lacking though, is some sort of Active Directory connection that allows you to import all your server objects directly, and not manually add/remove the serves as your infrastructure changes over time.

In an attempt to bridge that gap, I've made a very small PowerShell script that queries your Active Directory for server objects and dumps their names into a text file that you can import into RDCMan. This is a very simple solution, but works great in my environment. 



#### GetAllServers.ps1



    
    
    Import-Module ActiveDirectory 
    
    $servers = Get-ADComputer -LDAPFilter "(operatingsystem=*Windows Server*)" | select name,dnshostname
    $Date = Get-Date
    $filename = "Servers-{0}{1:d2}{2:d2}" -f $date.year,$date.month,$date.day
    foreach ($server in $servers) { 
     
    $servername = $server.name
    
    #Customize this for your environment
    $servername | Out-File -append -encoding ASCII <path>\$filename.txt
    
    }
    



In short, replace the path in **line 11**, and you should be able to run the script. It will then create a file called servers-{current-date}.txt in the path you have specified. This is a simple text file with one server defined on each line.

This file can then be imported into RDCMan by going to the _Edit_ menu and select _Import Servers_. This brings up  the _Import Servers_ dialog box where you can browse to the file that the PowerShell script created. Click on the _Import Button_ and all your servers should now be listed in RDCMan. The next time you need to update, delete the existing servers, re-run the PowerShell script and import again. 

[![](/img/RDCMan-1.png)](/img/RDCMan-1.png)
[![](/img/RDCMan-2-300x231.png)](/img/RDCMan-2.png)

While this isn't a fully automated solution, and I really wish RDCMan could do this for you by querying AD directly and finding new servers and removing the ones that are no longer present and so on, it is a quick way to get your current servers into RDCMan without manually creating each and every entry.



#### Update:


After I initially posted this, [Jan Egil Ring](http://twitter.com/#!/janegilring), pointed me to his solution which is a _bit_ more elaborate. Have a look at his solution "[Dynamic Remote Desktop Connection Manager connection list](http://blog.powershell.no/2010/06/02/dynamic-remote-desktop-connection-manager-connection-list/)", which is how it really should be done...

