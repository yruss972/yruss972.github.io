---
id: 37
title: CFEngine in mixed Solaris, Linux, environments
date: 2007-02-22T13:41:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2007/02/cfengine-in-mixed-solaris-linux-environments.html
permalink: /unix/cfengine-in-mixed-solaris-linux-environments.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2007/02/cfengine-in-mixed-solaris-linux.html
dsq_thread_id:
  - "522879129"
categories:
  - Tips
  - Unix
tags:
  - Advanced Packaging Tool
  - Blastwave.org
  - cfengine
  - debian
  - Dpkg
  - google
  - linux
  - Package manager
  - pkg-get
  - repository aware package management tools
  - Software distribution
  - solaris
  - solaris 10
  - sysadmin
  - System administration
  - Technology_Internet
  - ubuntu
---
If you&#8217;ve ever tried to use CFEngine for package management, you know that it is basically useless.

  1. CFEngine only supports installing packages and not removing them (yet).
  2. It has some crazy limit defined at compilation time which limits the number of packages you can install. (see Google://cfengine &#8220;Too many arguments in embedded script&#8221;)
  3. It merges all your package work into one logical section even if you <span style="font-style: italic;">logically</span> split up the sections and had them in a certain order etc.
  4. etc.

What comes out of all this is that you are better off using repository aware package management tools via the shellcommands actions (IMHO).

In the case of Solaris, the choice of champions seems to be pkg-get ala bolthole and blastwave.  
In the case of Debian/Ubuntu, use apt-get.  
This also means you no longer need to copy your package files using the cfengine copy actions since these tools will automatically retrieve them from your custom repository.

For those wanting to set up a pkg-get repository, the [makecontents script](http://www.bolthole.com/solaris/makecontents) should help you on your way.