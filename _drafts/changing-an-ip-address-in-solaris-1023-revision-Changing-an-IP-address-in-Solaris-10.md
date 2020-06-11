---
id: 110
title: Changing an IP address in Solaris 10
date: 2006-07-08T22:26:00-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2006/07/23-revision.html
permalink: /uncategorized/23-revision.html
---
I&#8217;m just starting to deal with Solaris 10 machines and I ran into a really annoying issue. When trying to change the IP address by modifying the appropriate files (or so I thought) the ip address kept getting reset on reboot.

It turns out that there is a IPV6 version of /etc/hosts called /etc/inet/ipnodes. I&#8217;m not sure why Solaris 10 puts the machine&#8217;s ip address in there but if you don&#8217;t get rid of it, your IP address won&#8217;t change.

[Google Groups : comp.unix.solaris](http://groups.google.com/group/comp.unix.solaris/browse_frm/thread/9371d7c3eeefc99e/8daac59cb3c46690?tvc=1&q=solaris+10+change+ip+address&hl=en#8daac59cb3c46690)