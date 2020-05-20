---
id: 246
title: Better User Management for MySQL
date: 2010-02-14T12:23:42-07:00
author: Yonah Russ
excerpt: |
  If you're like me, you love the granular permissions capabilities of MySQL but hate the work that goes into managing them...
  Unfortunately, there doesn't seem to be anything like groups in MySQL and according to plans won't be added officially until MySQL 7.0 (<a rel="nofollow" href="http://forge.mysql.com/worklog/task.php?id=988">WL#988</a>)...
  While searching around, I found <a rel="nofollow" href="http://code.google.com/p/securich/">Securich</a>...
layout: post
guid: http://www.yonahruss.com/?p=246
permalink: /tips/better-user-management-for-mysql.html
twitter_id:
  - "9108959789"
dsq_thread_id:
  - "527464925"
categories:
  - Tips
tags:
  - Cross-platform software
  - database table
  - File system permissions
  - group permissions
  - holding my breath
  - meta data
  - MySQL
  - MySQL 7.0
  - MySQL auditing
  - Oracle
  - privileges
  - readwrite
  - regexp
  - Securich
  - security
  - SQL Roles
  - sun
  - Technology_Internet
  - user management
---
If you&#8217;re like me, you love the granular permissions capabilities of MySQL but hate the work that goes into managing them.

Recently, I&#8217;ve been dealing with MySQL permissions a lot and most of the time I&#8217;m creating very similar permissions over and over again. It got me thinking that I could really use MySQL groups. Unfortunately, there doesn&#8217;t seem to be anything like groups in MySQL and according to plans won&#8217;t be added officially until MySQL 7.0 (<a rel="nofollow" href="http://forge.mysql.com/worklog/task.php?id=988">WL#988</a>). Considering they originally planned to include <a rel="nofollow"  href="http://bugs.mysql.com/bug.php?id=895">Role support in MySQL 5.0</a>, I&#8217;m not sure I&#8217;m holding my breath.

While searching around, I found <a rel="nofollow" href="http://code.google.com/p/securich/">Securich</a> &#8211; a project about 6 months old which uses stored procedures to create a much more capable and easy to manage permissions system on top of MySQL&#8217;s existing permissions. DISCLAIMER: I have not actually tried this so everything I say is based on what I&#8217;ve understood from the documentation.

On the simplest level, it gives you the ability to define roles as a set of privilege types. This isn&#8217;t what I had in mind but it does help. For example, there might be a role called &#8216;readonly&#8217; which only has SELECT permissions and there may be another role called &#8216;readwrite&#8217; which has &#8216;SELECT,INSERT,UPDATE,DELETE&#8217; etc. Then you can grant a role on tables or databases to users. Any time you update the permissions in the role, you update the permissions for all the users on every database/table where they have those permissions.

An interesting feature is the ability to define the tables in the grant privileges procedure using a regexp so if you use a prefix like &#8216;dev_ &#8216; to indicate tables used by developers, you might create a role called &#8216;developer&#8217; and apply it to all tables beginning with &#8216;dev_&#8217;.

Additional features I like are:

  * The ability to search for users with specific permissions.
  * The ability to clone users (ie. add another developer)
  * Storing meta-data on the users like contact email address.
  * Password aging, history,  and complexity requirements &#8211; although it seems like these are only enforced if the passwords aren&#8217;t modified using standard MySQL commands.
  * Auditing &#8211; Securich stores a history of when permissions are granted and to whom, etc.

What I don&#8217;t see:

  * I don&#8217;t see column level permissions
  * Preferably there would be a way to combine the permissions and the DB/Table set into the &#8216;Role&#8217; but the ability to clone users is pretty close.
  * Promised support &#8211; I&#8217;m a fan of open source but a one man project is not something for production. It would be nice if this were adopted by someone bigger.

To summarize, I don&#8217;t think I&#8217;ll be deploying this but it looks promising.  I hope Oracle will prioritize Roles  and deliver them before 7.0. If not,  maybe someone in MySQL will implement something similar to Securich (without the major architecture changes planned for 7.0 (pluggable authentication, new privilege table structures, etc.) to give us the quick win.