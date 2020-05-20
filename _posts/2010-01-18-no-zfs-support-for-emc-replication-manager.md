---
id: 213
title: No ZFS Support for EMC Replication Manager
date: 2010-01-18T13:43:19-07:00
author: Yonah Russ
excerpt: |
  As I originally blogged, I was hoping to use EMC snapshots to perform server-less/network-less backups...
  To make a long story short, Replication Manager is useless for LUNs with ZFS. According to EMC, this won't change in the near future. PowerSnap also has no support for taking snapshots of LUNs with ZFS on them so basically EMC has no server-less backup offerings for Solaris with ZFS...
  
  If EMC chooses not to support ZFS, they will only force us not to buy EMC arrays. We will stop buying disks, stop buying tools, etc.
  
  Instead, they should be providing better support for ZFS, integrating with ZFS to get better performance, providing tools which make EMC the preferred disk array behind a ZFS filesystem.
layout: post
guid: http://www.yonahruss.com/?p=213
permalink: /storage/no-zfs-support-for-emc-replication-manager.html
twitter_id:
  - "7917607813"
dsq_thread_id:
  - "522900130"
categories:
  - Storage
tags:
  - backup strategy
  - backup zfs
  - Computer storage
  - disk encryption
  - Disk file systems
  - emc
  - EMC Replication
  - Fault-tolerant computer systems
  - File system
  - Flash Cache technology
  - FreeBSD
  - heavy hardware
  - Intel
  - linux
  - Manager
  - Manager As
  - Manager EMC PowerSnap Networker Module
  - NetBSD
  - networker module
  - Nexenta OS
  - OpenSolaris
  - powersnap
  - raid
  - raid jbod
  - replication
  - replication manager
  - SAN
  - san storage
  - Snapshot
  - solaris
  - solaris 10
  - solaris admin
  - solaris backup
  - stop buying tools
  - storage area network
  - storage hardware
  - sysadmin
  - Technology_Internet
  - VMWare
  - zfs
---
As I originally [blogged](http://www.yonahruss.com/architecture/emc-replication-manager-in-solaris.html), I was hoping to use EMC snapshots to perform server-less/network-less backups. EMC provides two main tools for managing snapshots in this type of situation:

  * EMC Replication Manager
  * EMC PowerSnap Networker Module

The PowerSnap Module supposedly automates taking snapshots for the purpose of backups, while Replication Manager supposedly provides a much more robust package.

With Replication Manager you might create a policy to take a snapshot every five minutes, keep the last 10, and use those for backups whenever necessary.

To make a long story short, Replication Manager is useless for LUNs with ZFS. According to EMC, this won&#8217;t change in the near future. PowerSnap also has no support for taking snapshots of LUNs with ZFS on them so basically **EMC has no server-less backup offerings for Solaris with ZFS**.

As an IT guy in general, ZFS is the best thing that has happened to file systems in the last 10 years and it is only getting better. ZFS is already standard in FreeBSD and NetBSD. Linux supports ZFS over FUSE due to license issues but I&#8217;m confident those will be solved. The file system is platform independent, meaning you can move the data transparently between Intel and Sparc architectures. Deduplication has just been added to the feature set and disk encryption is on it&#8217;s way.

As a Solaris admin, I really can&#8217;t figure out why EMC would decide to cut off their own foot like this. It is clear that UFS will remain for legacy and backwards compatibility but ZFS is the future. Not planning to support ZFS is like not planning to support Solaris.

The only possibility that I can see is that EMC sees Sun, Solaris, and ZFS as enough of a threat, that they are strategically trying to limit options? For operations local to a server, ZFS has largely replaced the need for heavy hardware like EMC on the SAN. Some would argue that ZFS RAID + JBOD is better than ZFS + RAID on EMC. You can do the snapshots without the EMC. On a simple level, you can send snapshots asynchronously to another system, similar to MirrorView, without the EMC. You can do deduplication without the EMC. Now with Sun&#8217;s Flash Cache technology which integrates with ZFS, you can get the performance without the EMC. Along the same lines, you see Sun changing the rules of the storage/database game with solutions like Exadata V2. The integration of Zones with ZFS may be challenging Vmware on the virtualization front, especially with the serious advantage Sun&#8217;s Coolthreads servers have in terms of consolidation.

That said, I still prefer to offload this work to dedicated storage hardware for the time being and probably in the future. If EMC chooses not to support ZFS, they will only force us not to buy EMC arrays. We will stop buying disks, stop buying tools, etc.

Instead, they should be providing better support for ZFS, integrating with ZFS to get better performance, providing tools which make EMC the preferred disk array behind a ZFS filesystem.