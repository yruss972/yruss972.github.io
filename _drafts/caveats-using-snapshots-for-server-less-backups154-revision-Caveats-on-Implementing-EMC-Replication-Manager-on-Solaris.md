---
id: 155
title: Caveats on Implementing EMC Replication Manager on Solaris
date: 2009-11-16T12:01:51-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2009/11/154-revision.html
permalink: /tips/154-revision.html
---
Whether you are dealing with disk I/O in reading the data from the disks, or CPU for compressing or encrypting the data (or both- remember to compress and then encrypt!), or network for transferring the data to a backup server, the added load of a backup on your production servers is unwelcome. For this reason, the period of time during which backups can be made, aka. backup window, may be limited- even severely.

You may say, &#8220;It only takes me 3 hours to do a full backup of everything&#8221;, but over time backup windows are notorious for becoming too small. Backups are split over multiple days, technologies upgraded, etc. When planning a backup strategy, my approach is to eliminate the backup window altogether- that is do whatever you can to take the backup off the production hardware altogether.