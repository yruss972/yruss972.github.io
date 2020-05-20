---
id: 265
title: EMC Fully Automated Storage Tiering
date: 2010-10-13T16:00:19-07:00
author: Yonah Russ
layout: post
guid: http://www.yonahruss.com/?p=265
permalink: /architecture/emc-fully-automated-storage-tiering.html
dsq_thread_id:
  - "522877478"
categories:
  - Architecture
  - Storage
  - Unix
tags:
  - archive storage
  - automatic storage
  - Cache
  - clariion
  - Computer storage
  - Computer storage devices
  - Computing
  - Data management
  - device
  - emc
  - EMC Corporation
  - File system
  - Flash
  - Hierarchical storage management
  - IBM SAN Volume Controller
  - IOPS
  - less demanding applications
  - Manager
  - 'Manager- SAM and QFS'
  - non-volatile device
  - Oracle
  - Oracle Corporation
  - performance applications
  - qfs
  - raid
  - RAM
  - replication manager
  - s 7000
  - Solid-state drive
  - storage
  - storage community
  - storage devices
  - storage pool
  - sun
  - SUN CORPORATION
  - Technology_Internet
  - zfs
---
Storage Tiering is nothing new. We use fast 15K RPM disks for high performance applications, slower 10K RPM disks for less demanding applications, and 7.2K RPM SATA disks for archive storage. Recently, solid state disks (SSDs) have also become more common for really high performance needs. The trick is managing it all.

Two or three years ago, if you wanted to implement automatic storage tiering, I would have pointed you in the direction of Sun&#8217;s Storage and Archive Manager- SAM and QFS, Sun&#8217;s tightly integrated shared file system. SAM-QFS automatically moves files from one storage tier to another based on the SAM policy and transparently retrieves the files when requested. With tape still the least expensive storage available, this is still a great solution for archiving petabytes of documents/files.

Unfortunately, SAM works at the file level so it will not help our databases run faster. What will help us is ZFS. ZFS is still making some fairly big waves in the storage community with it&#8217;s Hybrid Storage Pool feature. In a standard configuration, ZFS uses RAM for a Layer 1 read cache (ARC).  In advanced configurations, the zpool can be configured to use a Layer 2 cache (L2ARC) on faster disks ie. SSDs compared to SAS compared to SATA , etc. The zpool can also be configured to use separate, possibly faster disks for the ZFS Intent Log (ZIL) which is basically a write cache (without getting into why it is more than a write cache). Even without faster disks, the ability to store the read/write cache on a separate device can increase performance just by dedicating more IOPS to the cause.

Oracle/Sun&#8217;s 7000 series storage builds on the success of the ZFS Hybrid Storage Pool, using Logzilla devices for the ZIL and Readzilla devices for the L2ARC. With the powerful flash acceleration in the storage pool, even 7.2K RPM disks can give performance equal to that of higher speed 15K RPM disks.

Although ZFS does great things for performance by utilizing multiple tiers of storage devices, all the data is still physically stored on the same tier of storage in addition to having the hot data stored again in the caches. This is arguably a waste of capacity but can also lead to performance issues in some cases. For example, a cold L2ARC cache after reboot could give slower performance until fully warmed up. Oracle will probably fix this at some point by allowing the L2ARC to persist if stored on a non-volatile device ([bug_id=6662467](http://bugs.opensolaris.org/bugdatabase/view_bug.do?bug_id=6662467)).

In the meantime, EMC recently announced an interesting new feature called FAST, short for Fully Automated Storage Tiering. FAST is available from FLARE version 04.30.000.5.004. FAST allows you to define a pool in the array composed of multiple RAID Groups, and then define a LUN on the pool as opposed to defining a LUN on the RAID Groups themselves. Once the LUN begins filling with data, the EMC will transparently begin transparently migrating data between the tiers of the pool in 1GB chunks, storing hot data on the fastest tiers and coldest data on the slowest tier.

FAST sounds like a dream come true. No more complicated storage configurations for the database. No more packages and processes to move historical data to slower disk groups. On the other hand, I am skeptical as to whether or not this technology is really mature. Do all EMC products treat the FAST LUNS the same as traditional LUNS (SnapView, Replication Manager, etc.) Also, are the ramifications of disk failures for a FAST LUN the same or does failure of a Tier 1 disk in a FAST pool mean alot more high performance eggs in one basket? Time will tell.