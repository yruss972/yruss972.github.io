---
id: 262
title: Replace faulty disk in SVM mirror
date: 2010-08-11T10:23:52-07:00
author: Yonah Russ
layout: revision
guid: http://www.yonahruss.com/tips/44-revision-3.html
permalink: /tips/44-revision-3.html
---
A disk I have in a production machine went bad:

> <pre>d4: Mirror
  Submirror 0: d14
    State: Okay
  Submirror 1: d24
    State: Needs maintenance
     Pass: 1
     Read option: roundrobin (default)
     Write option: parallel (default)
     Size: 120850176 blocks (57 GB)

d14: Submirror of d4
     State: Okay
     Size: 120850176 blocks (57 GB)
     Stripe 0:
          Device     Start Block  Dbase        State Reloc Hot Spare
          c1t0d0s4          0     No            Okay   Yes

d24: Submirror of d4
     State: Needs maintenance
     Invoke: metareplace d4 c1t1d0s4
     Size: 120850176 blocks (57 GB)
     Stripe 0:
          Device     Start Block  Dbase        State Reloc Hot Spare
          c1t1d0s4          0     No     Maintenance   Yes</pre>

The first thing I did was check iostat to see how bad the situation was:

> <pre>bash-3.00# iostat -En
...
c1t1d0           Soft Errors: 9 Hard Errors: 98 Transport Errors: 27
Vendor: SEAGATE  Product: ST373207LSUN72G  Revision: 045A Serial No: 060133PK2W
Size: 73.40GB &lt;73400057856&gt;
Media Error: 84 Device Not Ready: 0 No Device: 14 Recoverable: 9
Illegal Request: 0 Predictive Failure Analysis: 0...</pre>

98 Hard Errors doesn&#8217;t look good. (It was probably less the first time I noticed the problem.) Let&#8217;s do a surface scan: <span style="font-weight: bold;">format -> 1 -> analyze -> read -> y</span>

Without posting the output- suffice it to say that I need to replace the disk. To do this we will have to dettach it from the mirror and offline the disk. If your disk is also part of a ZFS pool, you will need to dettach it from there as well.

Assuming the bad disk is c1t1d0, this will break the mirror:

> <pre>for a in `metastat -c | grep c1t1 | awk '{print $1}'`;
     do A=`echo $a | sed 's/.$/0/'`;
     metadetach -f $A $a;
     metaclear -f $a;
done</pre>

You can use <span style="font-weight: bold;">zpool detach poolname device</span> to break any basic zfs mirrors.

Then delete any metadb&#8217;s that you have on the bad disk. This can be a little tricky. You want at least 3 dbs to remain. If you followed SUN&#8217;s advice and put 2 replica state databases on each of the two disks (SunFire v210) then you might want to add some more before you delete the ones on the bad disk. FYI: You cannot add db&#8217;s to a slice which already has DB&#8217;s on it.

Assuming the metadb&#8217;s are on slice 3, <span style="font-style: italic;">metadb -d c1t1d0s3</span> will delete them and leave you free to offline the disk.

> <pre><span style="font-weight: bold;">bash-3.00# cfgadm -al</span>
Ap_Id                          Type         Receptacle   Occupant     Condition
c0                             scsi-bus     connected    configured   unknown
c0::dsk/c0t0d0                 CD-ROM       connected    configured   unknown
c1                             scsi-bus     connected    configured   unknown
c1::dsk/c1t0d0                 disk         connected    configured   unknown
c1::dsk/c1t1d0                 disk         connected    configured   unknown
c2                             scsi-bus     connected    unconfigured unknown
usb0/1                         unknown      empty        unconfigured ok
usb0/2                         unknown      empty        unconfigured ok
<span style="font-weight: bold;">bash-3.00# cfgadm -c unconfigure c1::dsk/c1t1d0</span></pre>

At this point, a blue LED should light up next to the disk which needs to be replaced (at least it does in a V210, other hardware might be different). Replace the disk and get ready to undo everything we did 😉

> <pre><span style="font-weight: bold;">bash-3.00# cfgadm -c configure c1::dsk/c1t1d0</span>
<span style="font-weight: bold;">bash-3.00# format </span>
<span style="font-style: italic;"># Label the disk with format if necessary</span>
<span style="font-weight: bold;">bash-3.00# prtvtoc /dev/rdsk/c1t0d0s2 | fmthard -s - /dev/rdsk/c1t1d0s2
bash-3.00# metadb </span>
      flags           first blk       block count
   a m  p  luo        16              8192            /dev/dsk/c1t0d0s3
   a    p  luo        8208            8192            /dev/dsk/c1t0d0s3
   a    p  luo        16400           8192            /dev/dsk/c1t0d0s3
   a    p  luo        24592           8192            /dev/dsk/c1t0d0s3
<span style="font-weight: bold;">bash-3.00# metadb -a -c 4 c1t1d0s3
bash-3.00# metadb</span>
      flags           first blk       block count
   a m  p  luo        16              8192            /dev/dsk/c1t0d0s3
   a    p  luo        8208            8192            /dev/dsk/c1t0d0s3
   a    p  luo        16400           8192            /dev/dsk/c1t0d0s3
   a    p  luo        24592           8192            /dev/dsk/c1t0d0s3
   a        u         16              8192            /dev/dsk/c1t1d0s3
   a        u         8208            8192            /dev/dsk/c1t1d0s3
   a        u         16400           8192            /dev/dsk/c1t1d0s3
   a        u         24592           8192            /dev/dsk/c1t1d0s3
<span style="font-weight: bold;">bash-3.00# metastat -c</span>
d20              m  4.0GB d21  d21
          s  4.0GB c1t0d0s1
d10              m  4.0GB d11  d11
          s  4.0GB c1t0d0s0
bash-3.00# metainit d22 1 1 c1t1d0s1
d22: Concat/Stripe is setup
bash-3.00# metainit d12 1 1 c1t1d0s0
d12: Concat/Stripe is setup
bash-3.00# metattach d20 d22
d20: submirror d22 is attached
bash-3.00# metattach d10 d12
d10: submirror d12 is attached
<span style="font-weight: bold;">bash-3.00# metastat</span>
d20: Mirror         
   Submirror 0: d21
    State: Okay
   Submirror 1: d22
    State: Resyncing
  Resync in progress: 8 % done
  Pass: 1
  Read option: roundrobin (default)
  Write option: parallel (default)
  Size: 8392080 blocks (4.0 GB)

d21: 
Submirror of d20
  State: Okay
  Size: 8392080 blocks (4.0 GB)
  Stripe 0:
      Device     Start Block  Dbase        State Reloc Hot Spare
      c1t0d0s1          0     No            Okay   Yes

d22: Submirror of d20
  State: Resyncing
  Size: 8392080 blocks (4.0 GB)
  Stripe 0:
      Device     Start Block  Dbase        State Reloc Hot Spare
      c1t1d0s1          0     No            Okay   Yes

d10: Mirror
  Submirror 0: d11
    State: Okay       
  Submirror 1: d12
    State: Resyncing
  Resync in progress: 0 % done
  Pass: 1
  Read option: roundrobin (default)
  Write option: parallel (default)
  Size: 8392080 blocks (4.0 GB)

d11: Submirror of d10
  State: Okay
  Size: 8392080 blocks (4.0 GB)
  Stripe 0:
      Device     Start Block  Dbase        State Reloc Hot Spare
      c1t0d0s0          0     No            Okay   Yes

d12: Submirror of d10
  State: Resyncing  
  Size: 8392080 blocks (4.0 GB)
  Stripe 0:
      Device     Start Block  Dbase        State Reloc Hot Spare
      c1t1d0s0          0     No            Okay   Yes

Device Relocation Information:Device   Reloc  Device IDc1t1d0   Yes    id1,sd@SFUJITSU_MAW3147NC_______DAA0P7203F0Vc1t0d0   Yes    id1,sd@SFUJITSU_MAW3147NC_______DAA0P7203F1N</pre>

Don&#8217;t forget to rebuild your zfs pool if necessary.