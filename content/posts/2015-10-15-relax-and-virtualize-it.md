---
author: kwaeraas
comments: true
date: 2015-10-15 16:01:38+00:00
layout: post
link: http://vninja.net/virtualization/relax-and-virtualize-it/
slug: relax-and-virtualize-it
title: Relax and virtualize it!
wordpress_id: 3853
categories:
- Howto
- Microsoft
- Virtualization
- VMware
---

![2c10159](http://vninja.net/wordpress/wp-content/uploads/2010/07/2c10159.jpg)This is a guest post from [Kristian Wæraas](https://twitter.com/kwaeraas)
Senior Consultant Datacenter at [Datametrix AS](http://www.datametrix.no)
VMware VCP3/5, MCTS Hyper-V, Horizon View and Trend Micro Security Expert.



I am a curious by nature, and when my colleagues start talking frantic about some system that has crashed, I get curious and have to ask questions. Usually this ends up in me doing a lot of work.

**- This, however, was not one of those times.**

A few weeks ago, one of my colleagues came in late after a long night trying to fix a reoccurring bluescreen on a critical customer database server. Quite drawn in his face he sat down, picked up his phone and called Microsoft Support. I have to admit that I did some eavesdropping on that conversation, as it contained a few interesting tidbits that aroused my curiosity.

**-“Physical server” (We still use those?)**

**-“Database on FC SAN”.**

**-“Critical data!” (Oh my!)**

The minutes went by and turned into hours, and they still tried to fix the server. Diagnostics, rescue disks, rescue console, driver reinstalls, system file checking, fixing mbr and so on – but the server refused to cooperate. At some point Microsoft gave up on fixing the server, and asked if we could just reinstall it, which in this case would take even more hours.

When lunchtime came they had taken a break, and I started asking my colleague questions; not regarding the bluescreen and possible fixes, but more on the basic layout of the system. It turns out that it was an old physical server running Windows Server 2008 R2, it had Oracle database installed with the database-files placed on SAN mounted directly in to the server via FC - A normal setup for database servers I guess. We had a little chat on possible solutions to the problem during lunch, and my colleagues first thought was actually to find an identical physical server so he could install it parallel to the faulty server, then physically moving the fiber cable from the unstable server to the new. I of course asked if we could virtualize the server instead.

My colleague thought the idea was intriguing, but not knowing all the details in VMware’s possibilities had many questions.

_-“How will it perform, how do we get the database files copied into the server, how long will it take to get a server ready, we need at least 2 CPUs and 8GBs of RAM, will there be cake?”_

I explained to him that performance wise, the virtual server would do just fine, and that we could give it as much resources as it needed. As for getting a server up and running, I suggested using already prepared templates, which would take no more than a few seconds to deploy. Also, and this was my key point in this solution, the file copy is unnecessary:

_-“You don’t have to copy the files from SAN into the server; you can just do zoning on the FC switches, and directly attach the datastore as raw disks on the virtual server. The disks will then appear inside the OS as you are used to”._

_-“Is all this possible? How do we do it? If it is as easy as you say this would save us hours of work!”_

Having done similar setups before, I was quite confident. However “saving” a physical server with real critical data from humiliation by moving datastore into a virtual server was new to me, so I did a quick tweet to my good friend [Christian Mohn (vNinja extraordinare) ](http://twitter.com/h0bbel)to run my theory by him. We both agreed that the theory was spot on, but none of us had done this job before.

Being afraid of data loss, data corruption and the procedure in its whole, I agreed to do some tests to see if my theory was viable in our situation. We started with a basic SAN backup of the datastore, and then we did the necessary zoning by adding the backup LUN to the VMware host zone-group. After a quick rescan of datastores on the hosts, we saw the new LUNs available for the hosts.

[![1](http://vninja.net/wordpress/wp-content/uploads/2015/10/1-300x233.png)](http://vninja.net/wordpress/wp-content/uploads/2015/10/1.png)

The next thing was to add a new disk on a test-server, choose Raw Device Mappings (physical compability mode).

[![2](http://vninja.net/wordpress/wp-content/uploads/2015/10/2-300x235.png)](http://vninja.net/wordpress/wp-content/uploads/2015/10/2.png)

Found the correct LUNid

[![3](http://vninja.net/wordpress/wp-content/uploads/2015/10/3-300x235.png)](http://vninja.net/wordpress/wp-content/uploads/2015/10/3.png)

When all this was done, we logged into the test-server, went into Disk management and did a “Rescan Disk”. The disk appeared, drive-letter and all:

[![4](http://vninja.net/wordpress/wp-content/uploads/2015/10/4-265x300.png)](http://vninja.net/wordpress/wp-content/uploads/2015/10/4.png)

After verifying that the data was there, and that everything looked good, we felt confident that this approach worked, and we did the entire process again with the “live” data.

I always get a satisfied feeling inside when I am able to help a colleague solve an annoying issue. In this case, my actual work took no time at all; I also managed to open the eyes of my colleague who is now planning more p2v migrations. The customer was also happy, which in the end is what really matters.

I think the moral of the story is that “knowledge is power”. If you know what different solutions/products are capable of and you know how to use them correctly, you will be able to solve most problems quite quickly.

**And yes, there was cake!**
