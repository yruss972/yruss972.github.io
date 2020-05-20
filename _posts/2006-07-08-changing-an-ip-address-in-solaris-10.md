---
id: 23
title: Changing an IP address in Solaris 10
date: 2006-07-08T22:26:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2006/07/changing-an-ip-address-in-solaris-10.html
permalink: /unix/changing-an-ip-address-in-solaris-10.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2006/07/changing-ip-address-in-solaris-10.html
dsq_thread_id:
  - "523854015"
categories:
  - Tips
  - Unix
tags:
  - Domain name system
  - hosts
  - Internet Protocol
  - IP address
  - IPV6
  - solaris
  - solaris 10
  - Sun Solaris
  - sysadmin
  - Technology_Internet
---
I&#8217;m just starting to deal with Solaris 10 machines and I ran into a really annoying issue. When trying to change the IP address by modifying the appropriate files (or so I thought) the ip address kept getting reset on reboot.

It turns out that there is a IPV6 version of /etc/hosts called /etc/inet/ipnodes. I&#8217;m not sure why Solaris 10 puts the machine&#8217;s ip address in there but if you don&#8217;t get rid of it, your IP address won&#8217;t change.

[Google Groups : comp.unix.solaris](http://groups.google.com/group/comp.unix.solaris/browse_frm/thread/9371d7c3eeefc99e/8daac59cb3c46690?tvc=1&q=solaris+10+change+ip+address&hl=en#8daac59cb3c46690)