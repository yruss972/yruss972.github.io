---
id: 152
title: Webservd Default Home Directory
date: 2009-11-16T03:08:47-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/?p=152
permalink: /unix/webservd-default-home-directory.html
twitter_id:
  - "5761646728"
dsq_thread_id:
  - "531434349"
categories:
  - Unix
tags:
  - Business_Finance
  - google
  - home directory
  - java
  - java system
  - OpenSSO
  - Passwd
  - root user
  - security
  - solaris
  - solaris systems
  - Superuser
  - sysadmin
  - System administration
  - Technology_Internet
  - web container instance
  - Web Server
  - web server instance
---
Someone currently building an internal development environment required some integration between servers using SSH and the webservd user.

He came to me when he saw that the default home directory for the webservd user is /.  He didn&#8217;t want to create a /.ssh/authorized_keys file and I didn&#8217;t blame him. My first reaction was to change the home directory but I didn&#8217;t want to break something so I opened up Google and found something incredible.

DISCLAIMER: The following is quoted from documentation at <a rel="nofollow" href="http://docs.sun.com/app/docs/doc/820-3320/gfwox?a=view">docs.sun.com</a> (emphasis is mine). I do not recommend you actually listen to it&#8217;s instructions:

> If the runtime user of the OpenSSO Enterprise web container instance is a non-root user, this user must be able to write to its own home directory.
> 
> For example, if you are installing Sun Java System Web Server, the default runtime user for the Web Server instance is <tt>webservd</tt>. On Solaris systems, the <tt>webservd</tt> user has the following entry in the <kbd>/etc/passwd</kbd> file:
> 
> <tt>webservd:x:80:80:WebServer Reserved UID:/:</tt>
> 
> <span style="color: #ff0000;"><em><strong>The webservd user does not have permission to write to its default home directory (/). Therefore, you must change the permissions to allow the webservd user to write to its default home directory. </strong></em></span>Otherwise, the <tt>webservd</tt> user will encounter an error after you configure OpenSSO Enterprise using the Configurator.

Did someone actually write in documentation to give the webservd user write access to / ?!?!? What were they thinking?