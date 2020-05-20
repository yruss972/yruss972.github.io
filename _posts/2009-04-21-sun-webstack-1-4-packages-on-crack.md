---
id: 55
title: 'Sun Webstack 1.4 &#8211; Packages on Crack'
date: 2009-04-21T13:18:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2009/04/sun-webstack-1-4-packages-on-crack.html
permalink: /unix/sun-webstack-1-4-packages-on-crack.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2009/04/sun-webstack-14-packages-on-crack.html
dsq_thread_id:
  - "522879161"
categories:
  - Unix
tags:
  - apache
  - Coolstack
  - Filesystem Hierarchy Standard
  - linux
  - MySQL
  - SMF
  - solaris
  - solaris 10
  - solaris x86
  - Sun Microsystems
  - sysadmin
  - System administration
  - T2/T2+ processors
  - Technology_Internet
  - Webstack
---
I am a huge fan of Sun Microsystems.  
I love Solaris 10.  
I love ZFS.  
I love RBAC.  
I love zones.  
I really love T2/T2+ processors.  
I especially love the T5140 and X4450 servers.

One thing I cannot figure out though, is why Sun lets obviously delirious cocaine addicts package their software. Maybe I&#8217;m exaggerating but I think that many will agree that Sun&#8217;s packages leave much to be desired in general. On top of that, Sun seems to have a constant need to move software around and invent new paths- to boldy go where no sysadmin has gone before????

Our journey begins with the mythic /usr/ucb/ directory- a true treasure chest for those making the adjustment from Linux. We&#8217;ll continue to /usr/local/ ala sun freeware (actually the most normal place we will visit but not actually supported by sun) and then arrive at the more recent /usr/sfw.

On your right, we&#8217;ll be passing the Coolstack project (Not Officially Supported by Sun) located reasonably in /opt/coolstack. Notice the configuration files in /opt/coolstack/etc, apache located comfortably in /opt/coolstack/apache2, mysql located in /opt/coolstack/mysql. Can anyone guess where the SMF manifests are? My first guess would have been /opt/coolstack/var/svc&#8230; similar to the native manifests but I would be wrong because that would make too much sense or be too easy. Anyway- they are hiding in /opt/coolstack/lib/svc&#8230;

Wait- what&#8217;s that ahead? Coolstack is falling into disrepair, no longer to be updated. Instead, there will be a new neighborhood called Webstack and it WILL be officially supported by Sun- Time to get high. Can&#8217;t figure out where anything is? I&#8217;ll give you some hints:

Looking for configuration files? Don&#8217;t try /etc or /opt/webstack/etc. You should be looking in /etc/opt/webstack ??!?! Since when does that directory even exist?

Looking for your MySQL data directory? Don&#8217;t try /opt/webstack/mysql/data (similar to the existing structure in coolstack). Bet you wouldn&#8217;t have guessed /var/opt/webstack/mysql/5.0/data &#8211; /var/opt ??!?! What is that? Maybe for the 1.5 release they could put it in /usr/ucb/opt/usr/local/var/spool/sfw/webstack/mysql/5.0/data?

How about your default DocumentRoot for Apache? You must have guessed it by now: /var/opt/webstack/apache2/2.2/htdocs

Anyone here running webstack on Linux? In that case all the directories are different. I guess Sun wanted to make it difficult to run their stack on heterogenous environments?

Seriously- I really hope Sun wises up and fixes this before they hope for widespread adoption of the 1.5 release.