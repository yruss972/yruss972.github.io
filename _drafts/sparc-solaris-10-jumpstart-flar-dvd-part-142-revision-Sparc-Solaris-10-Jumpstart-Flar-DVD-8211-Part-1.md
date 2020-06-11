---
id: 98
title: 'Sparc Solaris 10 Jumpstart Flar DVD &#8211; Part 1'
date: 2007-06-27T01:50:00-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2007/06/42-revision.html
permalink: /uncategorized/42-revision.html
---
The Solaris Flash installation feature enables you to use a single reference installation of the Solaris OS on a system, which is called the master system. Then, you can replicate that installation on a number of systems, which are called clone systems. You can replicate clone systems with a Solaris Flash initial installation that overwrites all files on the system or with a Solaris Flash update that only includes the differences between two system images. A differential update changes only the files that are specified and is restricted to systems that contain software consistent with the old master image.

By combining Flash installation with Custom Jumpstart, and packaging all that on a re-mastered Solaris installation DVD, you can create very fast and efficient, standalone, and automated installation media.

I ran into several issues trying to create such a DVD when following the standard Google results so I thought I&#8217;d summarize my experiences. This is a work in progress- I might hit a brick wall at some point, but I hope not.

First, I built the prototype system. I&#8217;m running Solaris 10 11/06 with one non-global zone based entirely on a ZFS file system. This will make things challenging since Solaris Flash Archives are not completely compatible (or even supported) for these kinds of configurations and Jumpstart is not ZFS aware.

<span style="font-weight: bold;">Creating the Flash Archive</span>

  1. Make sure you have the right packages installed (SUNWinst, SUNWadmc, SUNWadmfw, SUNWbtool) Theoretically, you should install platform support for all possible hardware- I forget the name of the cluster- but if you will only be installing on the same hardware, this isn&#8217;t necessary. NOTE- If you try to install packages from inside single user mode with non-global zones it will give you issues.
  2. Put the prototype system into single user mode
  3. Create a text file, called for example &#8216;exclude&#8217;, with the directories not to include in the flash archive (man flarcreate)
  4. flarcreate -n system -X exclude -c system.flar  
    > <pre>Full Flash<br />Checking integrity...<br />Integrity OK.<br />Running precreation scripts...<br />Precreation scripts done.<br />Determining the size of the archive...<br />cpio: File size of "etc/mnttab" has decreased by 136<br />2259925 blocks<br />1 error(s)<br />The archive will be approximately 764.41MB.<br />Creating the archive...<br />2259925 blocks<br />Archive creation complete.<br />Running postcreation scripts...<br />Postcreation scripts done.<br /><br />Running pre-exit scripts...<br />Pre-exit scripts done.</pre>

  5. Verify your archive: flar info -l system.flar

More to come&#8230;