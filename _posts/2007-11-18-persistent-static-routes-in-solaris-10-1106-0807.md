---
id: 48
title: Persistent static routes in Solaris 10 11/06, 08/07
date: 2007-11-18T05:27:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2007/11/persistent-static-routes-in-solaris-10-1106-0807.html
permalink: /unix/persistent-static-routes-in-solaris-10-1106-0807.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2007/11/persistent-static-routes-in-solaris-10.html
dsq_thread_id:
  - "522879144"
categories:
  - Tips
  - Unix
tags:
  - cfengine
  - Configuration file
  - Cron
  - File system
  - Init
  - IPMP
  - Iproute2
  - network-based name resolution services
  - Route
  - Routing
  - Routing table
  - SMF
  - solaris
  - solaris 10
  - Static
  - Static routing
  - sysadmin
  - system startup
  - VPN
---
Static routes are a very common necessity once your networks become even a little complex. Whether you need to route specific traffic over a VPN or setup specific test addresses for IPMP failover, static routes are indispensable. 

For many years the &#8220;correct&#8221; way of configuring static routes in Solaris has been to create an init.d script which ran the &#8216;route add&#8217; commands.

As of Solaris 10 11/06, a more reasonable approach has been implemented. The &#8216;route&#8217; command has a new option &#8216;-p&#8217;. 

> Make changes to the network route tables persistent across system restarts. The operation is applied to the network routing tables first and, if successful, is then applied to the list of saved routes used at system startup. In determining whether an operation was successful, a failure to add a route that already exists or to delete a route that is not in the routing table is ignored. Particular care should be taken when using host or network names in persistent routes, as network-based name resolution services are not available at the time routes are added at startup.

Now you may be asking &#8220;Where is my configuration file?&#8221; The route command currently stores your static routes in the file /etc/inet/static_routes but this has been declared volatile. Sun is not promising to keep these configurations in that file or in the same format from release to release.

I personally am not happy with Sun&#8217;s general move to administrative utilities for configuration as opposed to configuration files. I agree that utilities are useful. They ensure correct syntax, etc. but I want the ability to configure a system on the file system level as well. Otherwise I loose the ability to keep a system&#8217;s configuration files in version control. I loose the ability to deploy a system by transferring the appropriate files (ala scp, cfengine, puppet, home grown script, etc.) I prefer something along the lines of crontab where the syntax is checked but the configuration itself is a file in userspace.

Still, a standard method for configuring static routes is welcome in place of creating init scripts, especially with SMF services phasing out init scripts altogether.