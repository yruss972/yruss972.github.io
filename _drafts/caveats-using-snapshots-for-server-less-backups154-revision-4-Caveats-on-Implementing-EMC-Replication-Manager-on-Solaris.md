---
id: 158
title: Caveats on Implementing EMC Replication Manager on Solaris
date: 2009-11-16T14:16:07-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2009/11/154-revision-4.html
permalink: /tips/154-revision-4.html
---
Whether you are dealing with disk I/O in reading the data from the disks, or CPU for compressing or encrypting the data (or both- remember to compress and then encrypt!), or network for transferring the data to a backup server, the added load of a backup on your production servers is unwelcome. For this reason, the period of time during which backups can be made, aka. backup window, may be limited- even severely.

You may say, &#8220;It only takes me 3 hours to do a full backup of everything&#8221;, but over time backup windows are notorious for becoming too small. Backups are split over multiple days, technologies upgraded, etc. When planning a backup strategy, my approach is to eliminate the backup window altogether- that is do whatever you can to take the backup off the production hardware altogether.

Storage Snapshots are one method for taking the production servers out of the backup equation. By creating a consistent, point in time snapshot on your storage, and mounting it on your backup server, you can backup your data using your backup server&#8217;s resources while your production servers continue as usual.

Caveats of this method in general are:

  1. Most snapshot technologies are some form of &#8220;Copy On Write&#8221;. This means that after you take a snapshot, the data from any area written to on the disks will first be copied somewhere else for safe keeping and then be overwritten. 
      * This may cause a performance hit on your production system as you are generating extra IO on every write.
      * As long as the data being used in production has not changed significantly from the snapshot, your backups will still be sending the majority of their read operations to the same physical disks being used by production so this doesn&#8217;t relieve the backup load on the storage as much as it relieves the load on the servers.
  2. Key word is &#8220;consistent&#8221;. 
      * You do not want to be <a rel="nofollow" href="https://bugs.edge.launchpad.net/ubuntu/+source/linux/+bug/317781">where KDE developers were when ext4 was released</a>. Depending on the applications or systems you are trying to backup, you may need to &#8220;quiet&#8221; them (FLUSH TABLES WITH READ LOCK,  ALTER TABLESPACE <tablespacename> BEGIN BACKUP, etc.)
      * If your application, ie. Oracle Database uses Datafiles or ASM spread over several LUNs, then all your storage level snapshots probably need to be taken together in order for the DB itself to remain consistent. For more, look at &#8220;Consistency Groups.&#8221;
  3. Once you have the snapshot, your backup server needs to see the snapshot LUN, and be able to mount the filesystem on the LUN. If your backup server doesn&#8217;t run the same operating system as your production servers this may be an issue. Ie. Try convincing a Windows server to mount a ZFS Pool (I dare you).