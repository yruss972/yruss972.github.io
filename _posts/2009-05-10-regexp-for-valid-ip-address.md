---
id: 56
title: Regexp for valid ip address
date: 2009-05-10T05:22:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2009/05/regexp-for-valid-ip-address.html
permalink: /tips/regexp-for-valid-ip-address.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2009/05/regexp-for-valid-ip-address.html
dsq_thread_id:
  - "557396221"
tags:
  - Classless Inter-Domain Routing
  - Internet Protocol
  - Internet Standards
  - IP address
  - IPV4
  - IVI Translation
  - perl
  - php
  - regexp
  - regular expressions
  - Routing
  - Sports
  - sysadmin
  - Technology_Internet
---
Hi,

Here is a fairly short regular expression you can use to match valid IPV4 IP addresses:  
/^(((25\[0-4])|(2[0-4\]\[0-9\])|(1?\[0-9]?[0-9]))\.){3}((25[0-4])|(2[0-4\]\[0-9\])|(1?[0-9]?[0-9]))$/

Good luck!