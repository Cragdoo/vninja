---
author: Christian Mohn
comments: true
date: 2016-10-14 18:57:20+00:00
layout: post
slug: me-too-vmware-cloud-on-aws
title: 'Me too: VMware Cloud™ on AWS'
url: /news/me-too-vmware-cloud-on-aws/
wordpress_id: 4260
categories:
- News
tags:
- AWS
- Cloud
- Speculation
- VMware
---

![](/img/VMware-AWS_750.png)After yesterdays announcement of VMware Cloud™ on AWS [everyone and their distant relatives](https://twitter.com/hashtag/VMWonAWS?src=hash) have published their opinion pieces on the relevance of the deal, and what who got the short end of the stick in this deal.  I guess this is my attempt, or me too post if you will.

<!--more-->

First off, the best source for actual facts and not conjecture, besides the press releases and [actual announcement](https://blogs.vmware.com/vsphere/2016/10/vmware-aws-announce-strategic-partnership.html),  is Frank Denneman's [VMware Cloud™ on AWS – A Closer Look](http://frankdenneman.nl/2016/10/13/vmware-cloud-aws-closer-look/) post (and by the way, it just _feels right_ that Frank is back at VMware).

<!--more-->

So far, not many details are available (naturally, since it's a tech preview that hasn't even entered the beta stage yet), but we know this:



  * It's not a nested environment, it's VMware SDDC running on physical hardware hosted in amazon data centers.


  * Your "old" management tools will be supported. Use VDP for backup, or PowerCLI to manage your environment? You can continue to do so, even if the workloads run in AWS.


  * vMotion will be supported, making it possible to migrate workloads directly.


  * AWS provides the hardware, VMware provides the stack and supports it.



So far, so good. Being able to run some of your services on AWS, without conversion, makes sense. But that's not really all that different (yes, there are probably subtle differences but still) from vCloud Air, Softlayer or any of the vCAN partners. There might be a difference in pricing, but that's unknown for now.

**My take on it is as follows:**

What has been announced now is basically a private cloud operating in a public cloud, enabling you to create a hybrid cloud. At the moment it's missing direct integration with existing aws services (other than workload proximity lowering latency). Elastic Scaling is the new name for Cloudbursting.

But as far as I can see, this is just the beginning. A lot of comments on this take the view that this is this is a short-term gain for VMware, but a long-term gain for aws. If anyone thinks that Jeff Bezos and Pat Gelsinger only thinks 6-12 months ahead, they need to get their head checked. This is a long term play by both parties, no doubt about it. Look at what VMware is doing with containers. Look at what aws is doing with their cloud services. Now tell me that their only thoughts are short-term.

I have no inside information about what is going on here, but Elastic Scaling (or ElasticDRS) sounds like it might be really interesting. The ability to move workloads to AWS on demand, if you exhaust your local resources? Price it like S3, with low cost availability of resources that costs (a lot) more if used. It's a move from CAPEX to OPEX anyway, and might make larger scale infrastructures easier to consume for the people in accounting.

Of course, this is aimed as a competing resource to Azure. That's the common enemy that makes AWS and VMware combine their forces. AWS has a massive advantage in cloud services, VMware owns most local datacenters.

And lets not kid ourselves, not all applications are going to be cloud applications. There are still a lot of mainframes out there, that in theory should have been replaced by x86 workloads years ago. The fact is that even though technology evolves, we still run around managing technical debt up the wazoo. Not having to run "legacy" x86 workloads in your local DC, is a big deal. In that regard the AWS deal offers nothing you couldn't already get through other avenues, but having the AWS name behind it ticks a lot of boxes.

The combination of these two, as the service offering evolve, is going to be interesting. Really, really interesting, and what has been announced so far is just the beginning.

I'll offer one small piece of advice in the end here, start learning AWS tools and infrastructure. You as a seasoned VMware veteran, or architect, will need it in the not so distant future. Trust me on this.
