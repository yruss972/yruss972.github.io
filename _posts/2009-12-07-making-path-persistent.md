---
id: 209
title: Making Path Persistent
date: 2009-12-07T22:17:56-07:00
author: Yonah Russ
excerpt: |
  You can make the executable search PATH variable persistent in several ways:
  <ol>
  <li>On the system level you can ...</li>
  <li>On a per user level, or for the user root, you can ...</li>
  </ol>
layout: post
guid: http://www.yonahruss.com/?p=209
permalink: /unix/making-path-persistent.html
twitter_id:
  - "6455299444"
dsq_thread_id:
  - "530852972"
categories:
  - Tips
  - Unix
tags:
  - bash
  - Computer file
  - Entertainment_Culture
  - Environment variables
  - Exec
  - executable search
  - executable search path
  - executables
  - home directory
  - ksh
  - linux
  - PATH
  - Rm
  - search terms
  - search terms making path
  - Setuid
  - several ways
  - solaris
  - solaris 10
  - sysadmin
  - tcsh
  - Technology_Internet
---
I&#8217;ve been paying a lot of attention to this site since I switched platforms and somehow people are finding some fairly irrelevant content on my site for the search terms **making path persistent in solaris 10** so I figured I better put some real answers up.

It is hard to know exactly what kind of path they had in mind- were they referring to the standard PATH variable which lists the directories in which to search for executables or were they referring to something more complicated?

You can make the executable search PATH variable persistent in several ways:

  1. On the system level you can set it in the /etc/profile file. It will affect all users except maybe root.
  2. On a per user level, or for the user root, you can set the PATH in the .profile file in the user&#8217;s home directory