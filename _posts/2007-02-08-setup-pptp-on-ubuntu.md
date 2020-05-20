---
id: 35
title: Setup PPTP on Ubuntu
date: 2007-02-08T06:52:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2007/02/setup-pptp-on-ubuntu.html
permalink: /unix/setup-pptp-on-ubuntu.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2007/02/setup-pptp-on-ubuntu.html
dsq_thread_id:
  - "523856729"
categories:
  - Tips
  - Unix
tags:
  - adsl
  - cable modem
  - Entertainment_Culture
  - GTK+
  - Internet Standards
  - ISP
  - linux
  - php
  - Point-to-Point Protocol daemon
  - Point-to-Point Tunneling Protocol
  - pptp
  - pptpconfig
  - sysadmin
  - Technology_Internet
  - Tunneling protocols
  - ubuntu
---
Here is a quick howto on installing and setting up PPTP on Ubuntu.  
Specifically I&#8217;ll be attempting to configure this machine to use the Israeli ISP 012 over a cable modem. 012 provides some sort of installation package for Linux but it doesn&#8217;t support Ubuntu.

Anyway- here are my steps:  
xhost +  
sudo su-  
export DISPLAY=&#8217;:0&#8242;  
echo &#8216;deb http://quozl.netrek.org/pptp/pptpconfig ./&#8217; >> /etc/apt/sources.list  
apt-get update  
apt-get install pptp-linux  
apt-get install pptpconfig  
pptpconfig&  
Use the server cablepns.012.net.il and the user/password provided by the ISP

Set the Cable connection to by your default route (All to Tunnel)

Select &#8216;usepeerdns&#8217; enabled (Automatic)

Set the tunnel to reconnect if disconnected.  
Use the following pppd options:

> noipdefault noauth default-asyncmap noipx defaultroute hide-password nodetach maxfail 1 lcp-max-configure 6 linkname cable ipparam cable-pptp userpeerdns persist mtu 1460 mru 1460 noproxyarp noaccomp nobsdcomp nodeflate nopcomp user cable lcp-echo-interval 20 lcp-echo-failure 3 

Click Add and Start  
&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8212;&#8211;  
Now if you don&#8217;t have a network connection which is very likely you will need your ubuntu cd and these files from the apt source we added:  
http://quozl.us.netrek.org/pptp/pptpconfig/php-gtk-pcntl\_1.0.0-2\_i386.deb  
http://quozl.us.netrek.org/pptp/pptpconfig/php-pcntl\_4.3.8-2\_i386.deb  
http://quozl.us.netrek.org/pptp/pptpconfig/pptpconfig\_20060821-0\_all.deb

Instead of the &#8216;apt-get install pptpconfig&#8217; step above do:  
dpkg -i php-gtk-pcntl\_1.0.0-2\_i386.deb  
dpkg -i php-pcntl\_4.3.8-2\_i386.deb  
dpkg -i pptpconfig\_20060821-0\_all.deb