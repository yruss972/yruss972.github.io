---
id: 109
title: showrev(1M) missing on Solaris 10
date: 2007-01-31T07:46:00-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2007/01/29-revision.html
permalink: /uncategorized/29-revision.html
---
Today I realized I was missing the showrev command on a Solaris 10 machine I installed.  
I found it in the SUNWadmc package but recieved the following error:

> ld.so.1: showrev: fatal: libadmapm.so.2: open failed: No such file or directory

Then I found the following page:  
[showrev(1M) missing on Solaris 8](http://www.grok.org.uk/docs/showrev.html)  
Adding the SUNWadmfw package as mentioned still left me with the following error:

> ld.so.1: showrev: fatal: libadmutil.so.2: open failed: No such file or directory

It turns out I was missing the SUNWadmlib-sysid package.

<span style="font-style: italic;">SUMMARY</span>

<span style="font-weight: bold;">If you are missing showrev check if you have the following packages:</span>  
<span style="font-weight: bold;">SUNWadmlib-sysid</span>   <span style="font-weight: bold;">SUNWadmc</span>    <span style="font-weight: bold;">SUNWadmfw</span>

Good luck!