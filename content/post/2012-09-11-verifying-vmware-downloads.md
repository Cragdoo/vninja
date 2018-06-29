---
author: cmohn
comments: true
date: 2012-09-11 21:19:12+00:00
layout: post
slug: verifying-vmware-downloads
title: Verifying VMware Downloads
url: /vmware-2/verifying-vmware-downloads/
wordpress_id: 2103
categories:
- VMware
tags:
- Bash
- Download
- Integrity
- MD5
- OS X
- SHA1
- Terminal
- VMware
- vSphere
---

Now that [vSphere 5.1](http://www.vmware.com/products/datacenter-virtualization/vsphere/index.html) and assorted products have been released into the wild, how do you check the integrity of your downloaded file? As you might be aware of, VMware publishes both **MD5** and **SHA1** hashes for their files, making it possible to check if the file you just downloaded is identical to the file offered from VMware.

Checking the **MD5** or **SHA1** hash for a single file is easy, at least in OS X where you don´t need any third party tools to check.

Open up _Terminal_ and navigate to your download directory and run either the _md5_ command, or the _shasum_ command to verify the file signature. You can then compare the signature to the checksums provided by VMware on the download page.

[![](http://vninja.net/wordpress/wp-content/uploads/2012/09/VerifyingDownloads-1-300x131.png)](http://vninja.net/wordpress/wp-content/uploads/2012/09/VerifyingDownloads-1.png)  [![](http://vninja.net/wordpress/wp-content/uploads/2012/09/VerifyingDownloads-21-300x57.png)](http://vninja.net/wordpress/wp-content/uploads/2012/09/VerifyingDownloads-21.png)

For a single file, this is not a problem, but what if you have downloaded a bunch of files into a directory and want to calculate the checksums for all of them in one go?

Thankfully this is pretty simple as well, just add a little Terminal (It´s really bash, I know) magic and you´re all set:

**MD5:**
[cci lang="bash"]find . -type f -exec md5 '{}' \;[/cci]

**SHA1:**
[cci lang="bash"]find . -type f -exec shasum '{}' \;[/cci]

These small bash one-liners will go through all the files in the current directory, and calculate checksums for each of them.

[![](http://vninja.net/wordpress/wp-content/uploads/2012/09/VerifyingDownloads-3-300x131.png)](http://vninja.net/wordpress/wp-content/uploads/2012/09/VerifyingDownloads-3.png)

_There are tools available for Windows as well that perform the same operations, but I haven´t looked into those for this post._


### A collection of MD5/SHA1 checksums for the vSphere 5.1 Release:


[table id=9 /]



As far as I can tell, VMware does not offer a single page that lists all of the checksums for their files, something that makes finding the checksums a bit tedious. I´ve found that the best way to find then, after you have downloaded the files, is to check the [My VMware Downloads History](https://my.vmware.com/group/vmware/my-downloads), page since it shows you all the downloads you have performed on one page, instead of going through multiple pages. Find the "_Show Checksums_" link to show the checksums, without having to open the download page for each item.

[![](http://vninja.net/wordpress/wp-content/uploads/2012/09/VerifyingDownloads-4-300x156.png)](http://vninja.net/wordpress/wp-content/uploads/2012/09/VerifyingDownloads-4.png)  [![](http://vninja.net/wordpress/wp-content/uploads/2012/09/VerifyingDownloads-5-300x176.png)](http://vninja.net/wordpress/wp-content/uploads/2012/09/VerifyingDownloads-5.png)


### Wishlist


I´m sure some scripting Wizkid could easily create a script that runs the checksum generation and then checks a given text file for a match, but I really wish VMware would create a service that enables you to easily check the checksums for a given file, without having to log on to VMware.com at all.

If we could do a simple HTML request to a service that returns the checksum for a given file, that would easily be scriptable and comparisons could be performed automatically.

That way, doing a request like this (yes, this is indeed a fake non-working URL):
[cci lang="bash"]curl vmware.com/integrity/md5/vCO_VA-5.1.0.0-817595_OVF10.cert[/cci]

would return a simple

    
    c636547afb4de258d9164a74ac622674


**Good idea? Bad Idea? Let me know!**
