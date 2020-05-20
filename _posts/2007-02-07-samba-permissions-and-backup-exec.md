---
id: 34
title: Samba Permissions and Backup Exec
date: 2007-02-07T12:18:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2007/02/samba-permissions-and-backup-exec.html
permalink: /unix/samba-permissions-and-backup-exec.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2007/02/samba-permissions-and-backup-exec.html
dsq_thread_id:
  - "526733608"
categories:
  - Storage
  - Tips
  - Unix
tags:
  - Backup
  - Backup Exec
  - Backup software
  - Business continuity and disaster recovery
  - IPC
  - samba
  - Symantec
  - sysadmin
---
I recently started using Symantec&#8217;s Backup Exec for Windows to backup files from a Samba share. I joined the Samba machine to the domain and gave the backup user permissions to the share but the backup kept getting access denied errors.

It turns out that Backup Exec doesn&#8217;t connect directly to the share (at least that&#8217;s not how our people set it up). It connects tot he IPC$ share and expects to find what to backup in the list of the machines shares.

Basically, I had to grant the backup user access to the IPC$ share as well. This tricky one slipped through testing since connecting directly to the share (ie. \\machine\share\) worked the whole time.