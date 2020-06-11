---
id: 256
title: 32 or 64 bit MySQL
date: 2010-07-12T05:23:48-07:00
author: Yonah Russ
excerpt: |
  Recently, I wanted to confirm that I was running the 64 bit version of the MySQL server as opposed to the 32 bit version... one of my admins had made a symlink from the mysql/bin directory to the 64 bit binary directory. On the command line, you could no longer tell if the mysqld command was run from the 64 bit directory and there doesn't seem to be a built in MySQL command which shows what version is currently running (show status, \s, show variables, etc)
  In the end I ran...
layout: revision
guid: http://www.yonahruss.com/tips/254-autosave.html
permalink: /tips/254-autosave.html
---
Recently, I wanted to confirm that I was running the 64 bit version of the MySQL server as opposed to the 32 bit version.  
The Sun Webstack installation comes with both versions and if you use the built in SMF service, the difference between using the 32 bit version or 64 bit version is controlled by a flag in the service properties.  
I was not using the built in service, but rather using Sun Cluster to start the server. In order to convince Sun Cluster to start the 64 bit version (I&#8217;m sure there is a better way to do this), one of my admins had made a symlink from the mysql/bin directory to the 64 bit binary directory. On the command line, you could no longer tell if the mysqld command was run from the 64 bit directory and there doesn&#8217;t seem to be a built in MySQL command which shows what version is currently running (show status, \s, show variables, etc)  
In the end I ran pldd on the process id of the MySQL server. I am reasonably sure that I am running the 64 bit version of the server because all the shared libraries being used came from the /lib/sparcv9/ and /usr/lib/sparcv9/ directories which are the 64 bit libraries. Not sure if this method works on other OS&#8217;s but I thought it might be helpful to someone.  
Good Luck!