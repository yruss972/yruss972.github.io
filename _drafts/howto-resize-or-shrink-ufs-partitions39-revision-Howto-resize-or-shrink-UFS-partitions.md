---
id: 99
title: Howto resize or shrink UFS partitions
date: 2007-03-15T03:28:00-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2007/03/39-revision.html
permalink: /uncategorized/39-revision.html
---
A friend of mine asked me the other day if there was such a thing as Partition Magic for Solaris. Apparently, someone had installed a system on a single slice and they&#8217;re security team was requiring a separate partition for the DB.

Here are the givens:

  * Sunfire V210
  * Solaris 8 (Otherwise we&#8217;d be using zfs)
  * 2 73GB disks
  * 1 slice on disk1
  * Disk 2 is supposed to be a mirror of disk 1 but it isn&#8217;t used yet
  * Downtime is allowed
  * Reinstalling is not an option

I personally don&#8217;t know of any tool that lets you shrink UFS partitions but that doesn&#8217;t mean that we can&#8217;t perform some <span style="font-style: italic;">Partition Magic</span> of our own.  
<span style="font-weight: bold;"><br />NOTE: </span>I have not tested this procedure. I think it is logical and should work and it should do no harm as the first disk remains fully intact.

  1. Go into single user mode
  2. Partition the second disk as required.
  3. <span style="font-weight: bold;">newfs </span>the partitions on the second disk
  4. Mount the second disk&#8217;s partitions
  5. Use <span style="font-weight: bold;">ufsdump</span>/<span style="font-weight: bold;">ufsrestore </span>to copy the filesystem into it&#8217;s smaller home  
    > `ufsdump 0f - / | ( cd /mnt/newroot ;ufsrestore xvf - )`

  6. When all the partitions are done, use <span style="font-weight: bold;">installboot </span>to make the second disk bootable.  
    > ``installboot /usr/platform/`uname -i`/lib/fs/ufs/bootblk /dev/rdsk/c1t2d0s0``

  7. Shutdown the system, physically swap the disks, and do a reconfiguration reboot.

If rebooting goes smoothly, test your new system thoroughly and then build your mirrors.