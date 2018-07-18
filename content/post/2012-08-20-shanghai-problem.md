---
author: eczerwin
comments: true
date: 2012-08-20 22:28:03+00:00
layout: post
slug: shanghai-problem
title: Shanghai...  we have a problem...
url: /virtualization/shanghai-problem/
wordpress_id: 1955
categories:
- Virtualization
tags:
- Disaster
- Fire
- Virtualization
---

My Saturday morning started the same as any other.  I checked my emails and my tweets, started a coffee, walked my dog and got into the shower.  My iPhone buzzing on the sink caught my attention a few minutes into it. Covered in soap and the rest censored for the public here I answered the call.  Without getting into too many details of my organization - my bosses-bossess-boss contacted me reporting **a fire** in one of our server rooms in Shanghai China. Trying not to panic I got it together and agreed to meet and discuss _ASAP_.



For privacy reasons lets cut to Monday. I drove to the Chinese Embassy that morning here in Zuerich and begged for a Visa as my plane was leaving at 19:00 that evening. They laughed initially since normally processing time is 7 days.  When they noticed the seriousness of the situation they told me to return in 1 hour and I would be granted a 1 year Visa.



Cut to Monday night - I flew from Zuerich to Charles De Gaulle in Paris had a few problems and ran across the entire airport but in the end made my flight. This is normal for changing planes in Paris :). After I got on the plane I shut myself down and forced myself to sleep because I knew I would have a big job on my plate when I arrived. I managed to get 4-6 hours of restless sleep and landed in Shanghai in the afternoon. I called the office and let them know about my arrival, they sent a car and the fun began.



When I arrived in the office I found** 10 seriously charred physical servers**.  Some with cut off melted power plugs and ethernet cables still in them.  I quickly asked them to place stickers on the servers that were priority and explain to me what exactly is the most important application/server to recover first. Again without getting into to much - our backups there were _"no longer available_"


[![](/img/Burned-Server-300x225.jpg)](/img/Burned-Server.jpg)




I managed to get a critical DB running again by copying the RAID config to disk right before it crashed again, switched the disks over to a loaner server and wrote the RAID config to the controller and quickly began a P2V to a new server I was provided that I installed vSphere 5 on when I arrived. This was only 1 of the many Hail Marys I was able to complete this week.



In the end of the week - 72 hours of work later, talking thru translators and a brief departure for some rest I was able to recover all but the oldest server.  I turned 10 physical servers into 2 vSphere 5 hosts with local storage, Better than nothing and flexible enough to change it later as needed.



**The moral of this story is in the face of disaster one of the best tools you have in your belt is Virtualization. You have flexibility that normally is not possible and can add more resources later as needed with minimal pain.  I know this goes back to basics but sometimes we need to go back to basics to really refresh our thoughts on the technology.**




