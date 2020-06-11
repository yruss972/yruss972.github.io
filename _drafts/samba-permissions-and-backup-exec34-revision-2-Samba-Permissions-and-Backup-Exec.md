---
id: 111
title: Samba Permissions and Backup Exec
date: 2009-11-08T13:58:34-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2009/11/34-revision-2.html
permalink: /uncategorized/34-revision-2.html
---
I recently started using Symantec&#8217;s Backup Exec for Windows to backup files from a Samba share. I joined the Samba machine to the domain and gave the backup user permissions to the share but the backup kept getting access denied errors.

It turns out that Backup Exec doesn&#8217;t connect directly to the share (at least that&#8217;s not how our people set it up). It connects tot he IPC$ share and expects to find what to backup in the list of the machines shares.

Basically, I had to grant the backup user access to the IPC$ share as well. This tricky one slipped through testing since connecting directly to the share (ie. \\machine\share\) worked the whole time.