---
id: 161
title: EMC Replication Manager in Solaris
date: 2009-11-16T15:29:58-07:00
author: Yonah Russ
excerpt: |
  While the data sheet claims support for Solaris, there are several caveats which I have run into.
  
  1. There is no mention of ZFS support in the data sheet and apparently, there is no support in the software either. One would expect this to be a non-question since ZFS has been part of Solaris since 2006.
  2. The data sheet is missing the word "SPARC" next to the word Solaris. There is no support for x86.
layout: post
guid: http://yonahruss.com/wordpress/?p=161
permalink: /architecture/emc-replication-manager-in-solaris.html
twitter_id:
  - "5777902392"
dsq_thread_id:
  - "522879182"
categories:
  - Architecture
  - Storage
  - Tips
  - Unix
tags:
  - Backup
  - backup server
  - backup servers
  - Backup software
  - clariion
  - Computer storage
  - Disk file systems
  - emc
  - EMC NetWorker
  - EMC Replication
  - Fault-tolerant computer systems
  - File system
  - GUI
  - linux
  - Manager
  - Manager in Solaris UPDATE
  - Manager in the near future Using storage level snapshots
  - raid
  - replication
  - replication manager
  - san storage
  - Snapshot
  - software working
  - solaris
  - solaris x86
  - sparc
  - sparc machines
  - sparc server
  - storage area network
  - storage level
  - Technology_Internet
  - zfs
---
**UPDATE: [No ZFS Support for Replication Manager in the near future](http://www.yonahruss.com/storage/no-zfs-support-for-emc-replication-manager.html)**

Using storage level snapshots can be used to run backups without directly requiring resources from the original host.

EMC Replication Manager coordinates the creation of application consistent snapshots across all the hosts in your network. It handles scheduling creation/expiration of snapshots,  mounting and unmounting from backup servers, etc. from a single console.

Although it is not tightly integrated into EMC Networker like the similar Networker PowerSnap module, it can be used to start a backup process after taking a new snapshot and it has the capability to manage snapshots unrelated to backups from a GUI.

While the data sheet claims support for Solaris, there are several caveats which I have run into.

  1. There is no mention of ZFS support in the data sheet and apparently, there is no support in the software either. One would expect this to be a non-question since ZFS has been part of Solaris since 2006.
  2. The data sheet is missing the word &#8220;SPARC&#8221; next to the word Solaris. There is no support for x86.

Honestly, this has put a dent in my plans since my backup server is an x86 box. I&#8217;m hoping the lack of ZFS support will work out as long as we can script any FS specific magic we need. I don&#8217;t have an option of running something like Linux on it (just to get the software working) because I won&#8217;t be able to even mount the ZFS filesystems- let alone back them up.

In the meantime, I&#8217;ll have to move my backups to a SPARC server and considering the lack of low end SPARC machines, I&#8217;ll have to allocate something way too expensive to be a backup server.