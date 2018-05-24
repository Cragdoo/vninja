---
author: esod
comments: true
date: 2015-11-10 19:30:20+00:00
layout: post
slug: vcenter-sso-unable-to-retrieve-ad-information-error-while-extracting-local-sso-users
title: vCenter / SSO unable to retrieve AD-information | Error while extracting local
  SSO users
url: /virtualization/vcenter-sso-unable-to-retrieve-ad-information-error-while-extracting-local-sso-users/
wordpress_id: 3879
categories:
- Virtualization
tags:
- '6.0'
- ESXi
- SSO
- vCenter
- vSphere
---

# 



After deploying a new VCSA 6.0u1 I was seeing some weird errors while trying to retrieve AD- users/groups (or anything from the esod.local domain):

![_1446154857693](http://vninja.net/wordpress/wp-content/uploads/2015/10/1446154857693.png)

After some serious head scratching, it dawned on me after checking the DNS records for the DC in the domain, from the vCenter Appliance itself:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
dig +noall +answer +search dc1.esod.local
dc1.esod.local. 3600 IN A 10.0.1.201
[/cc]

So far so good, the DNS lookup works as expected.

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
dig +noall +answer +search -x 10.0.1.201

[/cc]

That's right, the reverse lookup returns exactly zilch, zero, zippo, nil, nada and null.



# The Solution



Add reverse lookup zone to DNS and update the DC PTR record.![_1446155633910](http://vninja.net/wordpress/wp-content/uploads/2015/10/1446155633910.png)



Once that it done, it works as expected:

[cc lang="bash" width="100%" theme="blackboard" nowrap="0"]
dig +noall +answer +search -x 10.0.1.201
201.1.0.10.in-addr.arpa. 3600 IN PTR dc1.esod.local.[/cc]

Re-checking the domain in the vCenter Web Client, and  AD-information is retrieved correctly.

![_1446154788631](http://vninja.net/wordpress/wp-content/uploads/2015/10/1446154788631.png)



**It turns out that in VC6.0u1 reverse PTR records are required for SSO and Active Directory authentication to function properly.**
