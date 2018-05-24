---
author: cmohn
comments: true
date: 2017-04-15 22:00:08+00:00
layout: post
slug: psa-protect-your-email-with-dmarc
title: 'PSA: Protect Your Email with DMARC'
url: /misc/psa-protect-your-email-with-dmarc/
wordpress_id: 4650
categories:
- Misc
tags:
- dmarc
- email
- Security
---

In the last few months, I've seen an uptick in spoofed emails being sent with my own personal email domain. Not only is this extremely annoying, but more problematic is that recipients receive spam and phishing emails from what seems to be my personal mail account, simply by spoofing the from address. I don't know why domain and email address has been "chosen" for this, but I guess this is fallout from the [LinkedIn breach](https://www.troyhunt.com/observations-and-thoughts-on-the-linkedin-data-breach/) way back in 2012.

I didn't think there was much I could do about this, but a recent tweet by my friend [Per Thorsheim](https://twitter.com/thorsheim) sent me down the rabbit hole.

https://twitter.com/thorsheim/status/852099743908016128

https://twitter.com/thorsheim/status/852103821090279425

So, obviously there are options available to me that I was completely unaware of. I haven't managed any public facing email services for 6-7 years, so I've not kept up with whatever has been happening in that particular space. Also, my personal email domain has been hosted by Google since 2008, so I haven't really managed that either. Set and forget, right? Well, not quite.

So, what is this [DMARC](https://blog.returnpath.com/how-to-explain-dmarc-in-plain-english/) thing? It stands for _Domain-based Message Authentication, Reporting & Conformance,_ and is a way to try and validate that emails from a given domain is being sent using one of the valid mail servers configured for that domain. In order to be able to use DMARC, you first need to first have [Sender Policy Framework](https://en.wikipedia.org/wiki/Sender_Policy_Framework) (SPF) and [DomainKeys Identified Mail](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail) (DKIM) configured for you domain.

**Here are the resources I used to get all of this configured for my domain:**




    
  1. [Configure SPF records to work with G Suite](https://support.google.com/a/answer/178723?hl=en)

    
  2. [Authenticate email with DKIM](https://support.google.com/a/answer/174124?hl=en&ref_topic=2752442&visit_id=1-636278646523486675-1928954652&rd=1)

    
  3. [Add a DMARC record](https://support.google.com/a/answer/2466563?hl=en)



[![](http://vninja.net/wordpress/wp-content/uploads/2017/04/Screenshot-2017-04-15-23.41.40-300x247.png)](http://vninja.net/wordpress/wp-content/uploads/2017/04/Screenshot-2017-04-15-23.41.40.png)Less than 24 hours after configuring everything, I received my first [DMARC Aggregate Report](https://blog.returnpath.com/how-to-read-your-first-dmarc-reports-part-1/) which is basically an XML file showing what has been going on.

Since this file is a bit hard to read on it's own, I uploaded it to [DMARC Analyzer](https://app.dmarcanalyzer.com/guide), and even though I knew a lot of email was being send with my email address as the reply to address, I was quite surprised to see that in less then 24 hours after I set up the DMARC DNS records, **a total of 295 emails had been rejected by mail servers all over the world, most of them sent from mail servers in Vietnam. **I_ do not_ send 295 emails a day with my personal email account, and absolutely none of them from Vietnam. In fact, during the time-frame of this initial aggregate report, I sent zero emails - as seen in the screenshot from the report.

I have now configured my DMARC DNS txt records to send emails directly to  DMARC Analyzer, and I'm looking forward to seeing how these numbers add up over time. I'm currently on a free trial plan, and looking to evaluate which of the available DMARC Analyzers out there I want to use permanently.

At least now receiving email servers have a fighting chance of rejecting fake emails from my domain, since it's now possible to verify that they are sent through a valid source.

**Even if you don't have problems with someone spoofing your email addresses, please spend 10 minutes configuring this for your domain as well. You never know when something like this might occur, and it's better to build your defences _before_ you get attacked. That way you stand a chance of stopping it before it gets as ugly as it did in my case.**

And [Per](https://twitter.com/thorsheim), you are a gentleman and a scholar. Even if I did manage to investigate and set this up on my own, cake and coffee is still on me!
