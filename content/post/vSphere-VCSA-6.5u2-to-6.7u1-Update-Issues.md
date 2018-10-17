---
type: "post"
draft: "false"
date: "2018-10-17"
year: "2018"
author: Christian Mohn
topics: ["vCenter", "VMware"]
tags: ["VCSA", "VMware", "vCenter", "Blogtober2018"]

title: "vSphere VCSA 6.5 Update 2 to 6.7 Update 1 Upgrade Issues"

description: "Now that vSphere 6.7 Update 1has been released, I jumped at upgrading one of my lab environments from 6.5 Update 2 (which was not a supported upgrade path until 6.7 Update 1), and I pretty much immediately ran into issues. "

FeaturedImage: "https://vninja.net/img/vcsa-upgrade-error.png"

twitter:
card: "summary_large_image"
site: "@vninjanet"
creator: "@h0bbel" 
---

Now that [vSphere 6.7 Update 1](vmwa.re/vsphere67u1) has been released, I jumped at upgrading one of my lab environments from 6.5 Update 2 (which was [not a supported upgrade path](https://kb.vmware.com/s/article/53704) until 6.7 Update 1), and I pretty much immediately ran into issues. 

After providing all the details to the upgrade the wizard from the 6.7 Update 1 ISO, I was greeted by the following:

![VCSA 6.7 Update 1 Upgrade Error](/img/vcsa-upgrade-error.png#center)

The error log shows a somewhat unhelpful error:

{{< highlight plaintext >}}
ERROR
+ <Errors>
+ <Error>
+ <Type>ovftool.http.send</Type>
+ <LocalizedMsg>
+ Failed to send http data
+ </LocalizedMsg>
+ </Error>
+ </Errors>
{{< /highlight >}}

Naturally, my main suspect was DNS (It is *always* DNS. Or NTP.), but both forward and reverse DNS lookups works fine, both for my ESXi hosts as well as my existing vCenter. 

Unable to pinpoint this any further, I decided to try and use IPs for both the existing vCenter as well as the target host for my new VCSA (with the migrated data on it) and all of a sudden it worked fine.

**I do not really know what the problem is, besides it being related to DNS somehow, but the "fix" seems to be to use IP addresses for your environment in the Upgrade Wizard for VCSA.**

![VCSA 6.7u2 Upgrade Working](/img/vcsa-upgrade-working.png#center)

This is not really a fix, but it is a workaround and it does work. I would love to know why this was a problem, especially since both forward and reverse DNS lookups seem to work just fine in this environment.