---
id: 46
title: 'Solaris 08/07 &#8211; fmd broken on T2000'
date: 2007-11-01T03:50:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2007/11/solaris-0807-fmd-broken-on-t2000.html
permalink: /unix/solaris-0807-fmd-broken-on-t2000.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2007/11/solaris-0807-fmd-broken-on-t2000.html
dsq_thread_id:
  - "522879142"
categories:
  - Tips
  - Unix
tags:
  - Daemon
  - Extension
  - fault/diagnostic utilities
  - fmadm
  - Init
  - Kernel panic
  - patches
  - Process
  - sfm
  - solaris
  - solaris 10
  - Solaris Fault Manager
  - sysadmin
---
I recently installed Solaris 08/07 on two T2000 machines and was extremely surprised to find a serious bug with the fmd (Fault Management Daemon) service.

The service would, seemingly at random, fail to start on boot. It wouldn&#8217;t actually fail though- it just never finished starting. This caused numerous side effects including that prtdiag, fmdump, and other fault/diagnostic utilities would not work properly. It also seemed to cause problems moving between init levels.

You may have been bitten by this bug if you see some of the following:

> <pre>bash-3.00# fmadm  faulty<br />fmadm: failed to  connect to fmd: RPC: Program not registered</pre>

> <pre>bash-3.00# prtdiag -v<br />picl_initialize failed: Daemon not responding</pre>

> <pre>bash-3.00# svcs -xv<br />svc:/system/fmd:default (Solaris Fault Manager)<br /> State: offline since Mon Oct 08 15:35:25 2007<br />Reason: Start method is running.<br />   See: http://sun.com/msg/SMF-8000-C4<br />   See: man -M /usr/share/man -s 1M fmd<br />   See: /var/svc/log/system-fmd:default.log<br />Impact: This service is not running.<br /></pre>

This last output from `svcs -xv` might be normal if it doesn&#8217;t stay the same indefinitely. The `Start method is running.` should finish and the service should go online but if it stays in this state forever- you get the idea.

The next message may or may not be connected. I noticed it several times on boot in conjunction with the fmd failure to start. On the other hand, since the fmd failure caused problems with init levels, I had to sync the system from the ok prompt in order to power off the machine and this message might have been connected to the kernel panic from the previous shutdown.

> <pre>ds: [ID 406019 kern.notice] NOTICE: ds@1: invalid message length, <br />received 4128 bytes, expected 37536<br /></pre>

In the end this issue escalated it&#8217;s way back to Sun (after re-installing, re-installing from different media, switching disks, removing additional network cards, and disabling HW RAID, re-installing again, running explorer, realizing explorer didn&#8217;t say anything because prtdiag, etc didn&#8217;t work.

<span style="font-weight:bold;">Solution:</span>They fixed it with an upgraded OBP firmware which was released in October.