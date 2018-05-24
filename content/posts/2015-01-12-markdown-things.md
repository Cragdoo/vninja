---
author: cmohn
comments: true
date: 2015-01-12 23:31:45+00:00
layout: post
slug: markdown-things
title: Markdown All the Things!
url: /virtualization/markdown-things/
wordpress_id: 3483
categories:
- Virtualization
- Workflow
tags:
- Automator
- Content Creation
- GitHub
- Markdown
- OS X
- pandoc
- Workflow
---

Obviously I'm a bit late to the party here, but I guess late is better than never. It recently dawned on me that mucking about with lots of different file formats, interfaces and ways of doing things is rather counterproductive.

A lot of my work these days are related to generating content, be it a simple blog post like this or writing customer proposals and documentation. In the end, the deliverables are often quite different, but at the core they are strangely enough very similar. After all, the main thing is content, right? The file format itself, or how it is generated, doesn't really have a bearing on the content at all, it's just a delivery method. Lipstick on a pig, if you will.

So, in an effort to get rid of a lot of unnecessary visual and mental clutter , I've decided to go all in and **[Markdown](http://daringfireball.net/projects/markdown/) all the things.**

![](http://vninja.net/wordpress/wp-content/uploads/2015/01/58015307.jpg)

After all, Markdown is just text, with some simple formatting options. No fluff, no convoluted UI's, just text. Plain, simple, and very easy to work with. I currently use [atom.io](http://atom.io) as my preferred editor, which is working out very well so far.

Of course, from time to time my deliverables might just be a _.PDF_ file, or even a _Microsoft Word document_, but there are ways to work with that and still keep Markdown as the core content creation engine.

I've just put my first foray into [Automator](http://support.apple.com/en-us/HT2488) on [GitHub](https://github.com/h0bbel/automator-workflows). [The Auto-convert .md to .docx](https://github.com/h0bbel/automator-workflows/tree/master/autoconvert-docx/md2docx.workflow/Contents) is a simple [Folder Action](http://www.macissues.com/2014/07/16/how-to-set-up-and-use-folder-actions-on-your-mac/) that runs [pandoc](http://johnmacfarlane.net/pandoc/) to automatically convert files put into a given folder to _.docx_ format. It's simple and it's crude, but it works. Next I'll be looking into how I can use [pandoc](http://johnmacfarlane.net/pandoc/) with a Microsoft Word reference file, to make sure that the generated files adhere to the corporate templates I normally use, but for now this does the job. I also plan on extending this to convert to other file formats as I need them, now that I've got this first one up and running.

**Do you use Markdown or pandoc?** If so, please let me know how - I know I'm only scraping the surface of what is possible to do here, and any tips and hints on what do do next will be very welcome!

Now, if only [Evernote](http://evernote.com) would support Markdown natively...
