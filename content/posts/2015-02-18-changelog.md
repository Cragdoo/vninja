---
author: cmohn
comments: true
date: 2015-02-18 10:31:53+00:00
layout: post
link: http://vninja.net/workflow/changelog/
slug: changelog
title: Do you ChangeLog?
wordpress_id: 3598
categories:
- Workflow
tags:
- ChangeLog
- Ideas
- Markdown
- Workflow
---

In my work as a consultant I often have many small tasks to perform for customers, all while completing a bigger project. I have found that an easy way to keep track of all the little and big changes, is to create a ChangeLog. Normally ChangeLog's are referenced in development projects, but it also sense to use it to track of your own, or your team members, changes to an infrastructure environment.

As for [just about everything else](http://vninja.net/virtualization/markdown-things/), I use Markdown to make it easy to format and edit.



### Example



Currently, I use the following format to keep track of changes done to a customer environment


    
    
    # [customername]
    
    ## Changelog
    
    ### Completed
    | Date | Task | By |
    |-------------|---------------|-------|
    |14.02.2015|ChangeLog created| Christian Mohn
    |15.02.2015|Upgraded vCenter Operations Manager 5.8.3 Build 2076729 to 5.8.4 (Build 2199700)| CM
    |16.02.2015|Updated *vCSA01* from 5.5.0.20000 Build 2063318 til 5.5.0.20400 (Build 2442330)| CM
    |16.02.2015|Updated *esxi01* from 5.5.0 (Build 1623387) til 5.5.0 (Build 2456374) | CM
    |16.02.2015|Updated *esxi02* from 5.5.0 (Build 1623387) til 5.5.0 (Build 2456374) | CM
    |16.02.2015|Updated *esxi03* from 5.5.0 (Build 1623387) til 5.5.0 (Build 2456374) | CM
    



Once rendered as HTML, I get a nice list displaying all the recent changes.



### Completed



<table >

<tr >
  Date
  Task
  By
</tr>

<tbody >
<tr >
  
<td >14.02.2015
</td>
  
<td >ChangeLog created
</td>
  
<td >CM
</td>
</tr>
<tr >
  
<td >15.02.2015
</td>
  
<td >Upgraded vCenter Operations Manager 5.8.3 Build 2076729 to 5.8.4 (Build 2199700)
</td>
  
<td >CM
</td>
</tr>
<tr >
  
<td >16.02.2015
</td>
  
<td >Updated _vCSA01_ from 5.5.0.20000 Build 2063318 til 5.5.0.20400 (Build 2442330)
</td>
  
<td >CM
</td>
</tr>
<tr >
  
<td >16.02.2015
</td>
  
<td >Updated _esxi01_ from 5.5.0 (Build 1623387) til 5.5.0 (Build 2456374)
</td>
  
<td >CM
</td>
</tr>
<tr >
  
<td >16.02.2015
</td>
  
<td >Updated _esxi02_ from 5.5.0 (Build 1623387) til 5.5.0 (Build 2456374)
</td>
  
<td >CM
</td>
</tr>
<tr >
  
<td >16.02.2015
</td>
  
<td >Updated _esxi03_ from 5.5.0 (Build 1623387) til 5.5.0 (Build 2456374)
</td>
  
<td >CM
</td>
</tr>
</tbody>
</table>

As you can see this a quick and easy way to document changes. Since the markdown files are pure text files, they can easily be converted to other formats with Pandoc, or checked into a "code"-repository for easy retrieval.

**Do you use a ChangeLog for your infrastructure, or how do you quickly document changes in your environment?**
