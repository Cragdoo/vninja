---
author: Christian Mohn
comments: true
date: 2016-11-08 23:42:49+00:00
layout: post
slug: using-gitbook-for-secrets
title: Using GitBook for Secrets
url: /misc/using-gitbook-for-secrets/
wordpress_id: 4340
categories:
- Misc
tags:
- vDM30in30
topics: ["Markdown", "Gitbook", "Project"]

---

A while ago I decided to try and gather a bunch of non-public information in an easy to write and consume fashion. After a bit of fiddling, and testing different potential solutions, [GitBook](http://gitbook.com/) emerged as the best option. By using [MarkDown](https://daringfireball.net/projects/markdown/) as the markup language, it’s both cross-platform and easy to manage, as the content is nothing but raw text files.

<!--more-->


GitBook is awesome, no question about it, but in this case I didn't want the content hosted publicly on [gitbook.com](http://gitbook.com). Thankfully, GitBook is [available for download](https://github.com/GitbookIO/gitbook) as well, so I ended up running that locally on my MacBook. For details on how to run it locally, check out [Setup and Installation of GitBook](https://github.com/GitbookIO/gitbook/blob/master/docs/setup.md).

I then set up a private (free) repository on [Bitbucket](https://bitbucket.org/) to host the content.

So far this has been a very good experience. Writing content in Markdown is easy and quick, and running this locally makes it easy to check that the edits and additions look like expected in my browser.

{{< highlight bash >}}
$ gitbook serve dummy
Live reload server started on port: 35729
Press CTRL+C to quit ...

info: 7 plugins are installed
info: loading plugin "livereload"... OK
info: loading plugin "highlight"... OK
info: loading plugin "search"... OK
info: loading plugin "lunr"... OK
info: loading plugin "sharing"... OK
info: loading plugin "fontsettings"... OK
info: loading plugin "theme-default"... OK
info: found 13 pages
info: found 8 asset files
info: >> generation finished with success in 2.8s !

Starting server ...
Serving book on http://localhost:4000
{{< /highlight >}}

This way I can have my browser open http://localhost:4000 on one screen, and edit the content on my second screen while the browser auto refreshes.

[![screenshot-2016-11-09-00-37-02](/img/Screenshot-2016-11-09-00.37.02-1024x645.png)](http://slipsum.com/)

Once I've added some content I'm happy with, I push the changes back to the Bitbucket repository with my Git client. Once I'm happy with everything, GitBook makes it easy to create PDF and eBook versions for distribution.

We happy.
