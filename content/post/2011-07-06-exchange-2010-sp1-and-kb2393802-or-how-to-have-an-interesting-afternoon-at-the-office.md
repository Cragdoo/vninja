---
author: eczerwin
comments: true
date: 2011-07-06 12:51:21+00:00
layout: post
slug: exchange-2010-sp1-and-kb2393802-or-how-to-have-an-interesting-afternoon-at-the-office
title: Exchange 2010 SP1 and KB2393802 or How to Have an Interesting Afternoon at
  the Office
url: /microsoft-2/exchange-2010-sp1-and-kb2393802-or-how-to-have-an-interesting-afternoon-at-the-office/
wordpress_id: 1228
categories:
- Microsoft
tags:
- Event ID 2915
- Exchange
- KB2393802
- Messaging
- Microsoft
- Ops
- realworld
- Windows
---

Let me start this post out with a little story. I am normally a hardcore virtualization and storage guy. Sometimes my career in this sector brings me into working with stuff I haven't worked with before because virtualization encompasses so much. As I continue to work with other teams I learn more and more about what they do everyday. I usually find myself involved in every performance troubleshooting session and every new project these days. My personal philosophy is the IT guy of the future will be truly converged just as all the technologies are converging into 1 box or "stack". Specialties in smaller subsets will fall away and a more specialization in everything Datacenter may become the norm.

Early Monday morning I overheard a conversation about connection issues with our new Exchange 2010 environment while drinking some coffee and reviewing my brand new vSphere design. I didn't think about it very much until my boss came to my desk and asked me to have a look at the problem. Our messaging guy was on vacation and I was the only other person on staff who had some messaging experience. It seems that all of our global and even local offices were complaining about random exchange disconnections and also including email delivery delays from 30 minutes to 4 hours! It seems Activesync devices and OWA users were not affected by these delays at all. Being always up for learning new stuff I took the challenge.

First let's start with the quick facts I could put together. We had users in every country we have offices complaining about the random disconnections and delays. I had one actually confirmed in China but had some slight trouble getting exact user names from the local IT person. Also we had connections randomly disconnecting and showing disconnected in the lower right hand corner of the outlook client. I did not have any confirmation of who exactly was having these problems. To start I dug through the event logs on all the servers in the Exchange 2010 environment and the amount of errors I found was overwhelming. To shorten this up a bit and not write a novel most all of those I investigated were directly related to running Exchange 2010 SP1 without any update rollups in place. There were corresponding KB articles from Microsoft confirming these fixes in various update rollups.

I noticed an Event ID 2915 on our CAS servers that stuck out. I noticed several EWS and RPC connections reporting "Session Limit Over Budget". I correlated this with the Default Throttling Policy Exchange 2010 uses. It seems that the more mailboxes a user opens the more connections Exchange creates. It doesn't somehow truncate these connections. To understand more about the Default Throttling Policy see [Understanding Client Throttling Policies](http://technet.microsoft.com/en-us/library/dd297964.aspx). So I quickly whipped up a powershell script that set the Throttling Policy defaults to Null so there was no restrictions (funny Microsoft states as a workaround just to do this if you encounter an issue).
_If you are interested in seeing this script or want me to go deeper about Throttling Policies [contact me](http://vninja.net/about/ed-czerwin/), but this article isn't quite about this so I will move quickly on._

After the Throttling Policy was changed the reported disconnections stopped but the delivery delays continued as mentioned all around the globe. With the other problem out of the way I began to realize that the problem seemed very random. Some users experienced it, some not, some couldn't tell me whether they experienced it or not. This is when the hours of fruitlessly digging through configurations to learn them and reading about Exchange 2010 on Google began. I noticed our mailbox servers were set up in an active - active configuration with bidirectional replication using DAGs. This is when I decided to go back to basics of troubleshooting. I went over to my colleague sitting next to me and sent various test messages to him. All of them were promptly delivered without any problems. I noted down what server his mailbox was running on and moved on. Then I walked around the IT department until I was able to find a colleague that confirmed they had the delivery delays up to 4 hours. Just for kicks I turned off their cache mode on the Outlook client and their problem magically vanished. Then I turned cached mode back on and left it broken since I was determined to fix it on the server side and not just band aid the problem. When I went back to my desk and noticed what mailbox server the colleague with the delays was experiencing a light bulb went off and everything seemed to be coming together. Now all I had to do was note the differences between the 2 servers.

First of all to stop the global issue from occurring while I could resolve the problem I failed all the DAG volumes over to the 1 server that did not seem to be having the problem. Reports quickly came in that the problem was resolved. Then I quickly moved on to examining differences between the 2 servers. After comparing windows updates between boxes I noticed that some updates from February were recently applied to both servers, however, there was 1 difference. It seems [Microsoft KB2393802](http://support.microsoft.com/kb/2393802/en-us) was applied to one server but not the other. I googled regarding this but only found one vague thing about delays in Exchange 2010 mail delivery mentioned in the middle of a technet article relating to this patch, but nothing official at all from Microsoft. I removed the patch, rebooted, and tested with a test mailbox database running on the server I had created for this purpose. The problem was fixed as I thought.

**I tried to research on what about this patch could be causing this problem but came up with nothing. If any of you readers have an idea please comment and let me know your thoughts! I have attempted to contact Microsoft regarding this issue so they could possibly append to the KB article but they currently have not replied.**
