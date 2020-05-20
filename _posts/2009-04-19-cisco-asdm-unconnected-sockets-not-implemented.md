---
id: 53
title: Cisco ASDM unconnected sockets not implemented
date: 2009-04-19T04:54:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2009/04/cisco-asdm-unconnected-sockets-not-implemented.html
permalink: /tips/cisco-asdm-unconnected-sockets-not-implemented.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2009/04/cisco-asdm-unconnected-sockets-not.html
dsq_thread_id:
  - "522879150"
tags:
  - Applet
  - asa
  - asdm
  - cisco
  - Cisco ASA
  - Computing platforms
  - Cross-platform software
  - java
  - Java applet
  - Java Network Launch Protocol
  - Java platform
  - jnlp
  - jre 1.6
  - pix
  - Technology_Internet
  - web interface
---
Cisco ASDM recently started giving me the following error: _unconnected sockets not implemented._

After checking around, it seems that this is a known issue with newer Java releases, specifically the current version seems to require JRE 1.6u7.

Downgrading is an option but it is unnecessary. Instead open the Java control panel (Control Panel -> Java -> Java tab) there is a section &#8220;Java Application Runtime Settings&#8221;. Click <span style="font-style: italic;">View</span>.

This dialog controls which JVMs will be used when using JNLP (Java Network Launch Protocol). This is the technology behind the Cisco ASDM Java Applet. Uncheck the newer JVM versions and run the ASDM applet from the Cisco ASA web interface&#8230; this fix will not work with the ASDM Launcher.

This way you can still use the newer JVM for most applications, even re-enable/disable them for JNLP as needed.