---
id: 346
title: Linux and Solaris are Converging but Not the Way You Imagined
date: 2015-01-26T09:30:31-07:00
author: Yonah Russ
layout: post
guid: http://www.yonahruss.com/?p=346
permalink: /unix/linux-and-solaris-are-converging-but-not-the-way-you-imagined.html
categories:
  - Cloud
  - Unix
tags:
  - DTrace
  - Illumos
  - Joyent
  - linux
  - Linux Branded Zones
  - OpenSolaris
  - Operating-system-level virtualization
  - SmartOS
  - solaris
  - Technology_Internet
  - zfs
---
<img class="aligncenter size-full wp-image-347" src="/assets/images/2016/01/tux.jpg" alt="tux" width="698" height="400" srcset="/assets/images/2016/01/tux.jpg 698w, /assets/images/2016/01/tux-300x172.jpg 300w" sizes="(max-width: 698px) 100vw, 698px" />

In case you haven&#8217;t been paying attention, Linux is in a mad dash to copy everything that made Solaris 10 amazing when it launched in 2005. Everyone has recognized the power of Zones, ZFS and DTrace but licensing issues and the sheer effort required to implement the technologies has made it a long process.

## ZFS

ZFS is, most probably, the most advanced file system in the world. The creators of ZFS realized, before anyone else, that file systems weren&#8217;t built to handle the amounts of data that the future would bring.

Work to port ZFS to Linux began in <a href="https://events.linuxfoundation.org/sites/events/files/slides/OpenZFS%20-%20LinuxCon_0.pdf" target="_blank" rel="nofollow">2008</a> and a stable port of ZFS from Illumos was announced in <a href="https://groups.google.com/a/zfsonlinux.org/forum/?fromgroups=#!topic/zfs-announce/ZXADhyOwFfA" target="_blank" rel="nofollow">2013</a>. That said, even 2 years later, the latest release <a href="https://clusterhq.com/blog/state-zfs-on-linux/" target="_blank" rel="nofollow">still hasn&#8217;t reached feature parity</a> with ZFS on Illumos. With developers preferring to develop <a href="http://blog.delphix.com/prakash/2015/01/06/openzfs-on-illumos/" target="_blank" rel="nofollow">OpenZFS on Illumos</a> and the <a href="http://zfsonlinux.org/faq.html#WhatAboutTheLicensingIssue" target="_blank" rel="nofollow">licensing issues</a> preventing OpenZFS from being distributed as part of the Linux Kernel, it seems like ZFS on Linux (ZOL) may be doomed to playing second fiddle.

## DTrace

DTrace is the most advanced tool in the world for debugging and monitoring live systems. Originally designed to help troubleshoot performance and other bugs in a live Solaris kernel, it quickly became extremely useful in debugging userland programs and run times.

Oracle has been porting DTrace since at least <a href="http://dtrace.org/blogs/ahl/2011/10/10/oel-this-is-not-dtrace/" target="_blank" rel="nofollow">2011</a> and while they both own the original and have <a href="http://dtrace.org/blogs/ahl/2012/02/23/dtrace-oel-update/" target="_blank" rel="nofollow">prioritized the most widely used features</a>, they still <a href="http://dtrace.org/blogs/ahl/2014/12/27/dtrace-oel-dynamic-language-support/" target="_blank" rel="nofollow">haven&#8217;t caught up</a> to the original.

## Zones

Solaris Zones are Operating System level virtual machines. They are completely isolated from each other but all running on the same kernel so there is only one operating system in memory. Zones have great integration with ZFS, DTrace, and all the standard system monitoring tools which makes it very easy to support and manage servers with hundreds of Zones running on them. Zones also natively support a mechanism called branding which allows the kernel to provide different interfaces to the guest zone. In Oracle Solaris, this is used to support running zones from older versions of Solaris on a machine running a newer OS.

Linux containers of some type or another have been around for a while, but haven&#8217;t gotten nearly as mature as Zones. Recently, the continued failure of traditional hypervisors to provide bare metal performance in the cloud, coupled with the uptake of Docker, has finally gotten the world to realize the tremendous benefits of container based virtualization like Zones.

The current state of containers in Linux is extremely fractured with at least 5 competing projects that I know of. LXC, initially released in 2008, seems to be the favorite but historically had <a href="https://wiki.ubuntu.com/UserNamespace" target="_blank" rel="nofollow">serious privilege separation</a> issues but has gotten a little better if you can meet all the <a href="http://www.flockport.com/lxc-using-unprivileged-containers/" target="_blank" rel="nofollow">system requirements</a>.

## Joyent has been waiting at the finish line.

While Linux users wait and wait for mature container solutions, full OS and application visibility, and a reliable and high performance file system, Joyent has been waiting to make things a whole lot easier.

About a year ago, David Mackay showed some interest in Linux Branded Zones, work which had been abandoned in Illumos. In the spring of 2014, Joyent started work on resurrecting lx-zones and in September, they <a href="https://www.google.co.il/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0CCIQtwIwAQ&url=http%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DTrfD3pC0VSs&ei=HhvEVIbcCY7PaM26guAH&usg=AFQjCNE-TZOJwCGbGiGO3V0MPyvP2S0mLw&sig2=Y5RmM88SZqrxgi2QU-SX-Q&bvm=bv.84349003,d.d2s" target="_blank" rel="nofollow">presented their work</a>. They already have working support for 32 bit and some 64 bit Linux binaries in Linux branded SmartOS Zones. As part of the process, they are porting some of the main Linux libraries and facilities to native SmartOS which will make porting Linux code to SmartOS much easier.

The upshot of it is that you can already get ZFS, Dtrace, and Linux apps inside a fully isolated, high performance, SmartOS zone. With only 9 months or so of work behind it, there are still some missing pieces to the Linux support, but, considering how long Linux has been waiting, I&#8217;m pretty sure SmartOS will reach feature parity with Linux a lot faster than Linux will reach feature parity with SmartOS.