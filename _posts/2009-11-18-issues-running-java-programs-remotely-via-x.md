---
id: 165
title: Issues Running Java Programs Remotely via X
date: 2009-11-18T03:48:07-07:00
author: Yonah Russ
excerpt: |
  Recently I started using SmartSVN running remotely on a Solaris 10 server and displaying on my Windows XP machine via Xming (the free Xserver). I quickly ran into some performance and usability issues which I hope to have solved.
  
  There is an apparently known issue with running Java programs via X which causes all sorts of problems in painting the windows correctly. In my case, the mouse position was being shown in one place but the menus were being highlighted about 50 pixels lower.
  
  Uncommenting the <strong>export AWT_TOOLKIT=MToolkit</strong> did the trick.
layout: post
guid: http://yonahruss.com/wordpress/?p=165
permalink: /unix/issues-running-java-programs-remotely-via-x.html
twitter_id:
  - "5823331236"
dsq_thread_id:
  - "526672959"
categories:
  - Tips
  - Unix
tags:
  - awt toolkit
  - AWT_TOOL
  - Cursor
  - free xserver
  - Freedesktop.org
  - google
  - java
  - java command
  - Java Development Kit
  - Java platform
  - mouse position
  - MToolkit
  - NetBeans
  - performance
  - remote x
  - SmartSvn
  - Software development kits
  - solaris
  - solaris 10
  - svn
  - Swing
  - tunnel X
  - usability issues
  - windows xp
  - X servers
  - X Window System
  - X11
  - xming
  - XToolkit
---
Recently I started using SmartSVN running remotely on a Solaris 10 server and displaying on my Windows XP machine via Xming (the free Xserver). I quickly ran into some performance and usability issues which I hope to have solved.

  * **Performance:** 
      1. Xming uses OpenGL for rendering by default. I added the following option to the java command line: **-Dsun.java2d.opengl=true**  
        I&#8217;m not sure this technically made a difference but a developer claimed a 300% performance increase??
      2. Since I am working over a LAN, I disabled compression for the SSH connection being used to tunnel X and set my preferred cipher to Blowfish. Again- I&#8217;m not sure this made a difference but theoretically it should be faster.
  * **Usability:** 
      1. There is an apparently known issue with running Java programs via X which causes all sorts of problems in painting the windows correctly. In my case, the mouse position was being shown in one place but the menus were being highlighted about 50 pixels lower. It took me a while to find it in this post<a rel="nofollow" href="http://forums.java.net/jive/message.jspa?messageID=349144">about Swing on Remote X</a> and this related post about <a rel="nofollow" href="http://forums.sun.com/thread.jspa?messageID=10724065">cursors and menus in NetBeans</a>. Of course, after finding it in Google, I found the following comments in the SmartSvn start script:  
        > <pre># If you experience problems, e.g. incorrectly painted windows,
# try to uncomment one of the following two lines
#export AWT_TOOLKIT=MToolkit
#export AWT_TOOLKIT=XToolkit</pre>
        
        Uncommenting the **export AWT_TOOLKIT=MToolkit** did the trick. Now the menus are much more responsive and the cursors show in the correct position. </li> </ol> </li> </ul> 
        
        I&#8217;m not sure if the problem here is in XToolkit or in SmartSVN. I&#8217;m also not sure what this will mean in future versions of SmartSVN. Apparently JDK7 will not support MToolkit so either XToolkit has to work properly by then or SmartSVN has to be fixed to use XToolkit properly.