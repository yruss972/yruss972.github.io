---
id: 32
title: Cloning zones in Solaris 10 6/06
date: 2007-02-04T06:18:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2007/02/cloning-zones-in-solaris-10-606.html
permalink: /unix/cloning-zones-in-solaris-10-606.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2007/02/cloning-zones-in-solaris-10-606.html
dsq_thread_id:
  - "522879185"
categories:
  - Tips
  - Unix
tags:
  - apache
  - Cloning
  - Disk file systems
  - File system
  - Filesystem Hierarchy Standard
  - MySQL
  - OpenSolaris
  - Operating-system-level virtualization
  - php
  - raid
  - solaris
  - solaris 10
  - Solaris Containers
  - sysadmin
  - zfs
  - zones
---
I&#8217;m in the process of setting up a machine to host several SAMP (Solaris-Apache-MySQL-PHP) containers. I decided that it would be very efficient to create a generic zone and clone it over and over again. From reading up on the subject it seemed more than possible, after all, what is a zone besides a config file and a filesystem?

I googled for &#8220;Cloning Solaris Zones&#8221; and found lots of documentation on the zoneadm clone feature. I started to follow the howtos and hit a brick wall&#8230; my zoneadm doesn&#8217;t know how to clone. Deeper digging shows that the documentation on Sun&#8217;s site was for Solaris Express- Sun&#8217;s bleeding edge version of OpenSolaris- Can I say &#8220;How useless!&#8221;

I continued to google, after all I was very close I have the configuration and the filesystem, I just need to connect the two. I found the zoneadm attach/detach commands. This sounds perfect to me but alas my zoneadm doesn&#8217;t support attach/detach. Apparently, this feature is only available from Solaris 11/06- Can someone tell me when Sun started releasing new OS versions every 6 months!

I had no intention of giving up and here is the process which evolved:

  1. Setup the &#8220;Gold Master&#8221; zone including all the services, users, passwords, etc. (I&#8217;m assuming that your zonepath is a ZFS filesystem- this has it&#8217;s pluses and minuses so don&#8217;t take my word on it.)
  2. Halt the Master zone and export the config file to your zone template file:  
    > zoneadm -z master halt  
    > zonecfg -z master export -f /root/template

  3. It should look something like this: (edit with values for new zone)  
    > create -b  
    > <span style="font-weight: bold;">set zonepath=/zfszones/zoneclone</span>  
    > set autoboot=true  
    > <span style="font-weight: bold;">set pool=work1-pool</span>  
    > add inherit-pkg-dir  
    > set dir=/lib  
    > end  
    > add inherit-pkg-dir  
    > set dir=/platform  
    > end  
    > add inherit-pkg-dir  
    > set dir=/sbin  
    > end  
    > add inherit-pkg-dir  
    > set dir=/usr  
    > end  
    > add net  
    > <span style="font-weight: bold;">set address=192.168.0.2</span>  
    > set physical=bge0  
    > end  
    > add rctl  
    > set name=zone.cpu-shares  
    > <span style="font-weight: bold;">add value (priv=privileged,limit=10,action=none)</span>  
    > end

  4. Configure a new zone using the new config file:  
    > zonecfg -z zoneclone -f zoneclone.cfg

  5. Create a ZFS snapshot of the master zone:  
    > zfs snapshot zfspool/master@040207

  6. Clone the ZFS snapshot  
    > zfs clone zfspool/master@040207 zfspool/zoneclone

  7. Mount the new ZFS filesystem at the correct zonepath  
    > zfs setmountpoint=/zfszones/zoneclone/ zfspool/zoneclone

  8. Change the zone state to &#8220;installed&#8221; &#8211;<span style="font-weight: bold;">WARNING: I have no idea if this is a good idea but it seems to work.</span>  
    > vi /etc/zones/index
    > 
    > Find a line that looks like:  
    > zoneclone:configured:/zfszones/zoneclone:0000003c-ffbf-f825-ffbf-f80001000000
    > 
    > Replace it with:  
    > zoneclone:installed:/zfszones/zoneclone:0000003c-ffbf-f825-ffbf-f80001000000

  9. Boot the new zone:  
    > zoneadm -z zoneclone boot