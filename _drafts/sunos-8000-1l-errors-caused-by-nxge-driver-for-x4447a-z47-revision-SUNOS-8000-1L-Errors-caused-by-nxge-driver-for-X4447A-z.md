---
id: 94
title: SUNOS-8000-1L Errors caused by nxge driver for X4447A-z
date: 2007-11-02T04:26:00-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2007/11/47-revision.html
permalink: /uncategorized/47-revision.html
---
I recently installed Solaris 08/07 on a T2000 with a Sun Quad GbE x8 PCIe Low Profile Adapter (X4447A-z) inside. The machine gave lots of problems.

One of the issues was the following message which the machine logged hundreds if not thousands of times:

> <pre>Oct 23 22:18:27 hostname fmd: [ID 441519 daemon.error] SUNW-MSG-ID: SUNOS-8000-1L,<br /> TYPE: Defect, VER: 1, SEVERITY: Minor<br />Oct 23 22:18:27 hostname EVENT-TIME: Tue Oct 23 22:18:27 BST 2007<br />Oct 23 22:18:27 hostname PLATFORM: SUNW,Sun-Fire-T200, CSN: -, HOSTNAME: hostname<br />Oct 23 22:18:27 hostname SOURCE: eft, REV: 1.16<br />Oct 23 22:18:27 hostname EVENT-ID: 86cc16cc-a356-6a94-a11b-bbc8cd5e456f<br />Oct 23 22:18:27 hostname DESC: The EFT Diagnosis Engine encountered telemetry<br /> for which it is unable to produce a diagnosis.  Refer to<br /> http://sun.com/msg/SUNOS-8000-1L for more information.<br />Oct 23 22:18:27 hostname AUTO-RESPONSE: Error reports from the component will be<br /> logged for examination by Sun.<br />Oct 23 22:18:27 hostname IMPACT: Automated diagnosis and response for these<br /> events will not occur.<br />Oct 23 22:18:27 hostname REC-ACTION: Run pkgchk -n SUNWfmd to ensure that<br /> fault management software is installed properly. Contact Sun for support. </pre>

I originally assumed that these very descriptive messages were part of the same problem with the fmd service which I mentioned in a previous post but Sun found another source for the problem. Apparently it is the nxge driver.  
As I write this entry, Sun is working on a new driver. They tried a test version on my server and it <span style="font-weight:bold;">did not</span> solve the problem but it does seem to lessen the number of errors and add some information to the logs specifically, the entries above are sometimes preceded by a line similar to this:

> <pre>nxge: [ID 752849 kern.warning] WARNING: nxge2 : nxge_ipp_err_evnts: pkt_dis_max</pre>

In the meantime, it seems that I will be ditching the quad cards until Sun can get their act together. I&#8217;m getting them replaced by two dual gigabit cards which use the e1000g driver.