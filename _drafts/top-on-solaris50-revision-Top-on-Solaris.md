---
id: 92
title: Top on Solaris
date: 2008-11-16T11:10:00-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2008/11/50-revision.html
permalink: /uncategorized/50-revision.html
---
Recently, I was asked to give some advice on an integration project involving some Solaris web servers . One of the sides requested to install the top command.  
Now I know and love top for Linux but using top on Solaris is a waste in my opinion. Solaris comes with the prstat command built in- why use something else?  
Of course he answered that top was standard for him and he was used to it but I felt obliged to convince him otherwise so I dug around and found some proof 🙂

Brendan Gregg wrote up a great piece comparing top vs prstat using dtrace on his website:  
<a target="_blank" rel="nofollow" href="http://www.yonahruss.com/exit.php?url=www.brendangregg.com/DTrace/prstatvstop.html">http://www.brendangregg.com/DTrace/prstatvstop.html</a>[.](http://www.yonahruss.com/exit.php?url=www.brendangregg.com/DTrace/prstatvstop.html) 

In summary, he finds the following:

  * Top uses more system calls than prstat
  * Top opens and closes the psinfo file over and over while prstat only open it once and saves the file handle
  * Top takes more cpu time to do its job than prstat due to the overhead in the extra system calls and code differences
  * When top uses the cpu it uses it for longer than prstat
  * Most of the issues top has compared to prstat are connected to the number of processes running on the server so the more processes running, the worse top will perform compared to prstat

Aside from the performance issues, prstat also has the ability to give you project and zone related information which I doubt top knows about.

In short top is great for Linux but if you are going to use Solaris, use prstat!