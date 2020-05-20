---
id: 18
title: Mysql Replication Error
date: 2006-06-14T05:25:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2006/06/mysql-replication-error.html
permalink: /unix/mysql-replication-error.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2006/06/mysql-replication-error.html
dsq_thread_id:
  - "553206801"
categories:
  - Tips
  - Unix
tags:
  - Computer storage
  - Cross-platform software
  - Data synchronization
  - Disaster_Accident
  - Fault-tolerant computer systems
  - Hostname
  - MONyog
  - MySQL
  - Oracle Database
  - Replica
  - slave server
  - Technology_Internet
---
This helped when I changed the hostname on my mysql slave server:

Here is a sample error message:

> 060614 11:17:42 Error reading packet from server: Could not find first log file name in binary log index file (server_errno=1236)  
> 060614 11:17:42 Got fatal error 1236: &#8216;Could not find first log file name in binary log index file&#8217; from master when reading data from binary log

 <http://dev.mysql.com/doc/refman/5.0/en/replication.html>

[Neohapsis Archives &#8211; MySQL Discussion &#8211; #2000 &#8211; Re: weird replication problem with master.info being created empty](http://archives.neohapsis.com/archives/mysql/2004-q1/2000.html)