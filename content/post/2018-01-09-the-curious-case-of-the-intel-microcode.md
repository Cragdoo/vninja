---
author: bajorgensen
comments: true
date: 2018-01-09 18:37:24+00:00
layout: post
slug: the-curious-case-of-the-intel-microcode
title: The Curious Case of the Intel Microcode
url: /news/the-curious-case-of-the-intel-microcode/
wordpress_id: 4944
categories:
- News
tags:
- Analysis
- Intel
- Meltdown
- Security
- Spectre
---

This guest post by [Bjørn Anders Jørgensen](https://twitter.com/bajorgensen), Senior Systems Consultant Basefarm, first appeared on [LinkedIn](https://www.linkedin.com/pulse/curious-case-intel-microcode-bjørn-anders-jørgensen/).



**Disclaimer: This is a report based on current development as of 7. January, the situation is changing by the hour so read this opinion piece with that in hindsight.**



![](/img/spectre-text-252x300.png) ![](/img/meltdown-text-154x300.png)















Unless you have been living under a rock for the last week you will know by now that there is a universal design flaw in most modern microprocessors, leaving them vulnerable to a serious information disclosure problem that requires updates to all operating systems and processors.

If you are not familiar with the issue, start here: [Meltdown and Spectre](https://spectreattack.com)

[Jan Wildeboer](https://plus.google.com/+jwildeboer) also has a comprehensive timeline: [How we got to #Spectre and #Meltdown A Timeline](https://plus.google.com/+jwildeboer/posts/jj6a9JUaovP)

The issue has been known by Intel at least since June, and has been under embargo while everyone has been hammering out code to mitigate the threat and be ready when the embargo was lifted.

So we have three vulnerabilities, one that requires microcode update:

[CVE-2017-5715](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5715) (variant #2/Spectre) aka branch target injection

[Microcode](https://en.wikipedia.org/wiki/Microcode) is a small piece of code that can be loaded by the processor at boot time, either from the BIOS or from the operating system. Intel has been using this method for more than a decade to solve bugs and issues. It is a well known process. All HW vendors ships regular BIOS updates containing microcode updates and most modern operating systems has facilities to distribute and deploy microcode updates. This includes Linux, Windows and ESXi server. While most Linux distros keep an updated microcode repository, Microsoft and VMware has occasionally added microcode updates for serious issues.

So with everything blowing up this week, you'd think Intel would prepare microcode updates and ensure that OS patches contained the updated microcode so that you were completely secure from the know vulnerabilities. (more on the unknowns later)

No! We all have to wait for the HW vendors to release updated BIOS for all our computers. They are of course completely overloaded and will prioritize new models. Currently (7. Jan) on the server side, Dell has pretty good coverage for 13G and 14G servers and HPE has pretty good coverage for 9G and 10G servers. Anything older than 3 years are still vulnerable even after patching the OS/hypervisor. On the consumer side, it is all over the place.

Disclaimer: [The Register claims ](https://www.theregister.co.uk/2018/01/05/spectre_flaws_explained/)that variant #2/Spectre only requires microcode updates for Skylake and newer microarchitecture, in which case the tier 1 vendors already have good coverage, but the tier 2 and tier 3 vendors will lag for months. Even [Cisco is not planning BIOS updates](https://tools.cisco.com/security/center/content/CiscoSecurityAdvisory/cisco-sa-20180104-cpusidechannel) until 14. February!

Intel is as normal very, very quiet regarding microcode updates. They usually come with no or little release notes, and is basically treated like a black box, even though it becomes an integral part, changing critical functions in the CPU. The only content I've found was from an updated [Debian bug report](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=886367):


    
    New upstream microcodes to partially address CVE-2017-5715
         + Updated Microcodes:
           sig 0x000306c3, pf_mask 0x32, 2017-11-20, rev 0x0023, size 23552
           sig 0x000306d4, pf_mask 0xc0, 2017-11-17, rev 0x0028, size 18432
           sig 0x000306f2, pf_mask 0x6f, 2017-11-17, rev 0x003b, size 33792
           sig 0x00040651, pf_mask 0x72, 2017-11-20, rev 0x0021, size 22528
           sig 0x000406e3, pf_mask 0xc0, 2017-11-16, rev 0x00c2, size 99328
           sig 0x000406f1, pf_mask 0xef, 2017-11-18, rev 0xb000025, size 27648
           sig 0x00050654, pf_mask 0xb7, 2017-11-21, rev 0x200003a, size 27648
           sig 0x000506c9, pf_mask 0x03, 2017-11-22, rev 0x002e, size 16384
           sig 0x000806e9, pf_mask 0xc0, 2017-12-03, rev 0x007c, size 98304
           sig 0x000906e9, pf_mask 0x2a, 2017-12-03, rev 0x007c, size 98304
       * Implements IBRS and IBPB support via new MSR (Spectre variant 2
         mitigation, indirect branches).  Support is exposed through cpuid(7).EDX.
       * LFENCE terminates all previous instructions (Spectre variant 2
         mitigation, conditional branches).



Note the use of "partially". We'll get back to that in a minute. Breaking open the package, the changelog says:


    
    "unofficial bundle with CVE-2017-5715 mitigation"
    



So with six months lead time Intel has not been able to release an official microcode bundle update. The latest official one is from 17. November and does not seem to contain any Spectre fixes. Worse, it seems like the unofficial bundle is only partially fixing variant #2/Spectre.

Why is it that Intel is not using the tried and tested OS microcode update method to resolve what is arguably one of the most serious IT security issues in history? It could easily be included with the patch sets for the major OS vendors, like for Debian and other Linux distros. It has been done for VMware ESXi server and Microsoft Windows for less serious issues. And with the microcode revision that "partially address CVE-2017-5715" matching what the server vendors are releasing, do we have more updates coming?

Intel really needs to get a grip on this. These kind of side channel attacks are a completely new vulnerability category, and now that researchers are focusing on this, there will be more coming. As Anders Fogh said in a early [blog post](https://cyber.wtf/2017/07/28/negative-result-reading-kernel-memory-from-user-mode/):



<blockquote>"While I did set out to read kernel mode without privileges and that produced a negative result, I do feel like I opened a Pandora’s box. "</blockquote>



So we have possibly a partial patch currently and most likely new vulnerabilities coming down the road, I absolutely implore Intel to get a grip and prepare for a quick microcode update and distribution. There is no way that the server vendors will be able to release another BIOS update for a huge amount of servers. **The only sensible way is to use the OS update mechanism, even though that is not persistent and could potentially be tampered with. **And please, we need full disclosure. I suspect a lot of people believe they have resolved the vulnerability, while in fact they have not.

For virtual machines there are more steps required for the VM to enable the patch. More on that in a follow up post.

**Note:** We do have some unofficial Intel microcode repositories by [BIOS modders](https://www.win-raid.com/t3355f47-Intel-AMD-amp-VIA-CPU-Microcode-Repositories.html#msg45886) and [enthusiasts](https://vibsdepot.v-front.de/wiki/index.php/Cpu-microcode). While I applaud the effort and work, it really should not be necessary, and while the microcode is signed by Intel, modifying the inner workings of the CPU opens for absolutely undetectable persistent hacks and should only come from an official source.


