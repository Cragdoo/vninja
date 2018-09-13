---
type: "post"
draft: "false"
date: "2018-07-22"
year: "2018"
author: Christian Mohn
tags: ["vninja", "site", "news", "hugo", "wordpress"]
topics: ["Hugo"]
categories: ["site migration"]

title: "Migrating From Wordpress to Hugo"

description: "Migrating From Wordpress to Hugo aka The Long and Winding Road"

og_image: "https://vninja.net/img/hugo-logo.png"


twitter:
card: "summary_large_image"
site: "@vninjanet"
creator: "@h0bbel"
twimage: "https://vninja.net/img/hugo-logo.png"
---

<h1>
  Migrating From Wordpress to Hugo<br/>
 <small class="text-muted">aka The Long and Winding Road</small>
</h1>

When I decided to [move away from Wordpress](/2018/07/19/hello-my-name-is-hugo/) and over to [Hugo](https://gohugo.io), I really had no concept of the amount of work it would be. _Simply put; I just decided to do it — without really knowing what I set out to do._ 

Once I had decided on a foundation theme, I chose the [Hugo Bootstrap Premium](https://github.com/appernetic/hugo-bootstrap-premium/) as my starting point, the real work started.

The first challenge I had, was to get all the existing content moved over to Hugo. Hugo is entirely Markdown based, which ment I would have to find a way to export all the Wordpress posts in a format that Hugo could work with. After trying several Wordpress based plugins, that really didn't get the job done, either because the plugin itself had issues or that my hosting providers setup didn't match the requirements (or timed out while trying).


In the end, I decided to give [Exitwp](https://github.com/thomasf/exitwp) a try. In essence, [Exitwp](https://github.com/thomasf/exitwp) works with a "normal" Wordpress XML export, which meant that I could do an export, and then work with the exported data locally (for details, [check the documentation](https://github.com/thomasf/exitwp/blob/master/README.rst)). 
*Exitwp is designed for use with [Jekyll](http://www.jekyllnow.com), it works perfectly fine with Hugo as well.*

Basically what Exitwp does is to convert the existing Wordpress posts into Markdown files, with the publish date and post title as the filename (example: _2018-01-29-bootbank-cannot-be-found-at-path.md_)


This created a great foundation, as all the existing posts where now converted to Markdown, with existing metadata in the Front Matter. 

After conversion, my posts Front Matter looked like this example:

```
---
author: cmohn
comments: true
date: 2018-01-29 22:49:53+00:00
layout: post
slug: bootbank-cannot-be-found-at-path
title: Alerting on "Bootbank cannot be found at path ‘/bootbank’" in vRealize Operations
url: /vmware-2/bootbank-cannot-be-found-at-path/
wordpress_id: 4963
categories:
- VMware
tags:
- Alerting
...
---

```

Great! The **url:** parameter there is fantastic, as it means that old URLs would still work as before, even Hugo generates a new URL for them, with my preferred _/:year/:month/:day/:filename/_ format (because I believe easiliy accessible date information is pretty crucial). **Win!** Also, note how easy it is to have multiple URLs pointing to one post in Hugo! Also, if you want more, there is always the [Alias:](https://gohugo.io/content-management/urls/#example-aliases) parameter.

I then did a quick Search and Replace in all the files, [replacing _cmohn_ with _Christian Mohn_](https://github.com/h0bbel/vninja/commit/1340b3bebe9a8a486ca7f4c87e397afe221806c3). I don't really need the comments or wordpress_id metadata there anymore, but there's really no harm in it being there, so I left them.

<p class="lead text-center">Once that was done, I copied all my posts from the Exitwp conversion directory, to my Hugo /content/posts directory and all my posts showed up in Hugo locally! <strong>Awesome!</strong></p>

Everything looked OK, until I realized that all the image references were pointing to the existing absolute URL, which worked fine locally (as they pointed to the still running Wordpress site when I ran the local Hugo server) but that would break spectacularly once once I pointed the domain to the new location. I'm glad I caught that one before cutting over the domain! Sadly this was a real pain to fix, due to the following issues:

* My general non-proficiency in regexp.

* Wordpress is kind of stupid when it comes to uploads as it places uploaded files into _wp-content/uploads/{year}/{month}/{filename}_.

This made it hard to do a general search and replace for all the image references, so I pretty much went through each end every post [fixing and cleaning up the image URLs](https://github.com/h0bbel/vninja/commit/cb3e9d9f494c49fe7a6f4d37253467c7ce230e2f).

Once that was done, I also noticed that there was quite a lot of other Wordpress specific things I needed to clean up. All posts I had with code snippets in them needed to be fixed, as they were littered with Wordpress Plugin specific code tags. All posts that had images with captions also needed ~~deletion~~ cleanup, any Wordpress galleries as well as all embeds that I had (Twitter, Youtube etc.)

The next thing I did was to go parse through the image files in my /img/ folder, and [optimize them](https://github.com/h0bbel/vninja/commit/6d92638132bba394ae98c31df4ff65f61ebd993f). I used [jpegoptim](https://github.com/tjko/jpegoptim) and [optipng](http://optipng.sourceforge.net/) to accomplish that:

#### jpegoptim
```
find . -name "*.jpg" -exec jpegoptim -m80 -o -p --strip-all {} \;
```

#### optipng
```
find . -name "*.png" -exec optipng -o7 {} \;
```

By running this on my new img directory, which took quite a while on a total of >2.5k files,  I got a reduction of ~47 % or 187 MB, from 393,9 MB to 206,6 MB, without any noticeable quality degradation.

And that just about covers it, my Wordpress to Hugo migration, a short story long.

