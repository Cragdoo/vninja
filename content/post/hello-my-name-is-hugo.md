---
type: "post"
draft: "false"
date: "2018-07-19"
year: "2018"
author: Christian Mohn
tags: ["vninja", "site", "news", "hugo", "wordpress"]
categories: ["site migration"]

title: "Hello, My Name Is Hugo (Montoya)"

description: "Goodbye Wordpress, hello Hugo! vNinja.net has been powered by Wordpress since it was launched back in 2010, and frankly it was time for a change."

twitter:
    card: "summary_large_image"
    site: "@vninjanet"
    creator: "@h0bbel"
    title: "Hello, My Name Is Hugo (Montoya)"
    description: "Goodbye Wordpress, hello Hugo!"
    image: "https://vninja.net/img/hugo-logo.png"

---




# Goodbye Wordpress, hello Hugo!

vNinja.net has been powered by Wordpress since it was launched back in 2010, and frankly it was time for a change. What _really_ triggered it, was my hosting providers unwillingness to upgrade basic things, like PHP versions and MySQL. That combined with the security issues you have to live with when using traditional shared hosting made me look around for alternatives. While looking at hosting options, it slowly dawned on me that perhaps I should be looking at doing [something completely different](https://www.youtube.com/watch?v=FGK8IC-bGnU).

After looking around a bit, I started playing with [Hugo](http://gohugo.io/), and it just kind of stuck with me. 

_The allure of everything being static, just plain old files [written in Markdown](/virtualization/markdown-things/) was just impossible to resist._

So, what you're accessing right this very moment, is the new vNinja, in its new Hugo powered static glory over [HTTPS-only](https://blog.chromium.org/2018/02/a-secure-web-is-here-to-stay.html)! 

[![](/img/hugo-logo.png#center)](http://gohugo.io/)

When I discovered that I could combine [Hugo](http://gohugo.io/), [Github](https://github.com/h0bbel/vninja) and [Netlify](https://www.netlify.com/) it just clicked. I can now edit — and preview — locally, wherever I am, check in changes to GitHub and the [Netlify's Continous Development](https://www.netlify.com/docs/continuous-deployment/) takes care of updating the site automatically: 

![Netlify deploy](/img/Screenshot 2018-07-20 00.21.30.png#center)

I've even created a Slack-bot that notifies me of the status of the checking and publishing, to make sure I catch it when I mess things up:

![Go Live!](/img/Screenshot 2018-07-20 00.19.08.png#center)

Added bonus: Being able to use whatever editor I fancy to edit this thing, at the moment [Visual Code](https://code.visualstudio.com/) looks like it will do the job _just_ right.


I've spent quite a few hours playing around, exporting all the existing vNinja content out from Wordpress and convert them over to the new Hugo format (and writing a new [about page](/about/christian-mohn/)), and I've tried to make all old URLs work. I've updated quite a few posts with proper Markdown syntax, and I think I've managed to get most of the kinks ironed out. **If you notice anything that looks weird, let me know via [Twitter](https://twitter.com/h0bbel).** 

There is a few things I know isn't working, or missing:

* **Search is missing**, I'll add that later — Sneak preview [here](/search)
* **Author pages** for guest authors, and how to handle multiple authors/guest posts in general.
* **Various theme tweaks** — it's a work in progress. [Please provide feedback](https://twitter.com/h0bbel), if you have it.
* Favicon is not working right now, need to intestigate that.
* Not quite HTTPS only at this point, seems I might have to change some things to make that happen.

The theme itself is a work in progress, expect changes or breakage. Or both. 

I'll do a writeup of the [Wordpress to Hugo conversion process](/2018/07/22/migrating-from-wordpress-to-hugo/), and challenges I met, as well as the Netlify setup later on. For now, I just wanted it to go live.



