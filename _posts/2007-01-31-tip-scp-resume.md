---
id: 30
title: 'Tip: scp Resume'
date: 2007-01-31T23:33:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2007/01/tip-scp-resume.html
permalink: /tips/tip-scp-resume.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2007/01/tip-scp-resume.html
dsq_thread_id:
  - "594229086"
categories:
  - Tips
tags:
  - Data synchronization
  - rsync
  - scp
  - Secure copy
  - sysadmin
---
I left a desktop to download some humongous logs last night using scp and of course the connection was lost in the middle.

I looked for a possible resume feature in scp and found this tip:  
Using rsync to resume partial file transfers: [joen.dk » Tip: scp Resume](http://joen.dk/wordpress/?p=34)

rsync &#8211;partial &#8211;progress -e &#8216;ssh&#8217; &#8230;.