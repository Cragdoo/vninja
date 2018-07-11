---
author: cmohn
comments: true
date: 2017-10-24 13:47:35+00:00
layout: post
slug: integrating-pocket-with-todoist-via-ifttt
title: Integrating Pocket with Todoist via IFTTT
url: /workflow/integrating-pocket-with-todoist-via-ifttt/
wordpress_id: 4805
categories:
- Workflow
tags:
- IFTTT
- Pocket
- Todoist
- Workflow
---

[Myles Gray](https://twitter.com/mylesagray/status/922783625241866241) asked me how I integrate Pocket with Todoist, after my [How I use Todoist](http://vninja.net/workflow/how-i-use-todoist/) post, and the answer is very simple: [IFTTT](https://ifttt.com/). If-This-Then-That lets you connect services, and create rules (or applets) that trigger based on events in those services, luckily both Todoist and Pocket are supported.

Now, there is a bit of overlap between how I use Pocket and Todoist, but I mainly use Pocket to keep track of links I want to either read later, or use as basis for blog posts.
<!--more-->

[caption id="attachment_4807" align="alignright" width="644"][![](/img/kari-shea-199320-644x429.jpg)](https://unsplash.com/@karishea) Photo by Kari Shea on Unsplash[/caption]

I have two main IFTTT recipes that takes care of my integration between the two. Both of these use Pocket as the source, and Todoist as the target, I do not transfer anything _from_ Todoist _to_ Pocket.



## **IFFT Applets:**





### **"If new item tagged read, then create a task in To Read"**



Simply put, if I tag something with the tag **read** in Pocket, it gets added to my "_To Read_" sub-project in Todoist. This allows me to quickly move a Pocket item into Todoist as an action item, with the complete URL. It does not assign a label, nor does it set a priority—but it allows me to have a nice link list in Todoist with items I want read later. Of course, the Todoist Chrome extension allows me to do similar things, but only from the browser. Since I use IFTTT to add my Twitter likes to Pocket etc, it makes sense to have most of that collected in one place for further investigation.



### **"If new item tagged todo, then create a task in Inbox"**



Similar to the one above, just with the **todo** tag instead. Naturally those links get placed in my "To Do" project instead. The difference here is that those that go into my To Do project, are things that I want to actively do something with (besides reading). That might be create a blog post based on something, send it to a client or coworker, or similar tasks.



## Workflow



For me, Pocket works as a first repository of content I want to check out, and the content I want to do something further with, they live in Todoist. Once they are in Tooist, it's trival to move them over to the correct projects and/or labels for organizing.

I have also set up a recurring task, with mobile alerts, to make sure I check my Pocket-lint at least once a week.

I'm sure there are other, and more fancy ways of doing this, or even improve on them. Please leave a comment if you do something similar, or something that I haven't even thought of at all.
