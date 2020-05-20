---
id: 51
title: Network Interface Utilization in Solaris
date: 2008-11-19T05:49:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2008/11/network-interface-utilization-in-solaris.html
permalink: /unix/network-interface-utilization-in-solaris.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2008/11/network-interface-utilization-in.html
dsq_thread_id:
  - "522879149"
categories:
  - Unix
tags:
  - Advanced Packaging Tool
  - Aspect-oriented programming
  - Debuggers
  - DTrace
  - Form
  - kstat
  - linux
  - network load
  - network utilization
  - OpenSolaris
  - perl
  - solaris
  - solaris 10
  - solaris load
  - sysadmin
  - USD
---
A friend asked me how he could see the network utilization in Solaris. It seems like a fairly simple request but for some reason this is not a simple command line away.

In Linux I would instinctively go straight to _iptraf_. I don&#8217;t know if _iptraf_ is the tool of choice these days but I&#8217;m pretty sure it is an apt-get away if not already installed.

If you are a DTrace wizard, you could whip something up. Maybe you could get the information from one of the of the DTraceToolkit scripts if their installed. The DTraceToolkit scripts I&#8217;ve seen seem to give too much information as most of them are concentrated on not only telling you if the network is loaded but what is loading it as well.

For the sake of practice I wrote the following script:

> <pre>#!/usr/bin/perl -w<br />print "Interface: ";<br />$if=&lt;>;<br />chomp($if);<br />$max=`dladm show-dev -p $if | awk -F= '{print \$3}' | awk '{print \$1*1024*1024/8}'`;<br />print "Max speed: ",$max,"\n";<br />$if=~m/([a-z0-9]+?)(\d+)/;<br />($module,$instance)=($1,$2);<br />$last_rbytes=0;<br />$last_obytes=0;<br />while(1){<br />@kstat=`kstat ${module}:${instance}:mac:/[or]bytes\$/ |awk '{print \$2}'`;<br />chomp(@kstat);<br />if($last_rbytes!=0){<br />  printf("%02d%%\n", <br />         (($kstat[$#kstat-1]-$last_rbytes)+<br />          ($kstat[$#kstat-2]-$last_obytes))/$max*100);<br />}<br />$last_rbytes=$kstat[$#kstat-1];<br />$last_obytes=$kstat[$#kstat-2];<br />sleep 1;<br />};</pre>

This script will ask you which interface you want to watch and then print out the utilization percentage on a new row every ~second.

On a side note, it seems strange to me the the received bytes are stored in kstat as _rbytes_ while the transmitted bytes are stored in _obytes_. The only answer I can come up with is that if they would have chosen _ibytes_ (in bytes) instead of _rbytes_, then the &#8216;i&#8217; and &#8216;o&#8217; might become interchanged in typos since they are next to each other on the keyboard. If they would have chosen _tbytes_ (transmitted bytes), the same situation occurs- &#8216;r&#8217; next to &#8216;t&#8217;. Still, as a friend pointed out, they could have used _sbytes_ (sent bytes) which makes more sense than _obytes_.