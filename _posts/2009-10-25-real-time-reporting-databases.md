---
id: 60
title: Real Time Reporting Databases
date: 2009-10-25T17:11:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2009/10/real-time-reporting-databases.html
permalink: /architecture/real-time-reporting-databases.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2009/10/real-time-reporting-databases.html
dsq_thread_id:
  - "543081985"
categories:
  - Architecture
tags:
  - BI
  - business intelligence
  - Computer storage
  - cross database replication solutions
  - Cross-platform software
  - Data management
  - data warehousing
  - Database
  - database software
  - DBMS
  - Distributed computing architecture
  - ETL
  - high availability
  - Multi-master replication
  - MySQL
  - Operational database
  - Oracle
  - Oracle Corporation
  - Oracle Database
  - Pentaho
  - replication
  - reporting
  - Technology_Internet
  - Tungsten Replicator
---
Reporting projects are the kind of projects which never seem to end. After a couple iterations I&#8217;ve come to the following conclusions:

  1. Absolutely no reports should run on a production database.
  2. Moving/aggregating data from a production database to a reporting database using ETL tools prone to synchronization issues and pretty unreliable.
  3. The best option is to set up real time replication of the data and build additional views on that.

Unfortunately, if you need to get data from heterogeneous databases, ie. Oracle, MySQL, SQL Server, etc. into a single reporting database, replication is not a simple solution. If you are running expensive database software in production, it may not be cost effective to run the same database for reporting.

Of course there are cross database replication solutions like Golden Gate or SharePlex but they are very expensive. I had already given up on getting data from Oracle into MySQL for reports when I stumbled across [Tungsten Replicator](http://www.yonahruss.com/exit.php?url=www.continuent.com/community/tungsten-replicator).

According to the website, Tungsten Replicator provides open source database-neutral master/slave replication. Master/slave replication is a highly flexible technology that can solve a wide variety of problems including Cross DBMS Integration, ie. replication from Oracle to MySQL.

I&#8217;m looking forward to testing this product in the near future and I&#8217;d be happy to get anyone&#8217;s input if they&#8217;ve used it.