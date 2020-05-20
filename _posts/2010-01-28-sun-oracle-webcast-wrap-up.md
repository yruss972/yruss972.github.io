---
id: 218
title: Sun Oracle Webcast Wrap Up
date: 2010-01-28T13:15:05-07:00
author: Yonah Russ
excerpt: |
  Last night I watched almost the entire 5 hour live webcast announcing Oracle's strategies regarding the Sun Microsystems acquisition. As a near-evangelist for Sun and Solaris, I'm very happy with the deal finally going through and even happier that most of what Oracle said makes sense to me as a customer.
  
  Obviously, it is easy to  get up and say everything will integrate but doing it is much harder. Just getting past the internal politics of this will be a major issue. Now we can only wait and see if Oracle can pull it off.
  
  What I liked, disliked...
  
  I used to get upset with "Oracle people" for always thinking that Oracle was the solution to every problem. If they pull off this acquisition, I much just become an "Oracle person" myself.
layout: post
guid: http://www.yonahruss.com/?p=218
permalink: /architecture/sun-oracle-webcast-wrap-up.html
twitter_id:
  - "8336600963"
dsq_thread_id:
  - "522879180"
categories:
  - Architecture
tags:
  - California
  - designed server
  - emc
  - Enterprise Manager
  - Larry Ellison
  - ldoms
  - NetApp
  - Newark
  - NONEXISTANT
  - ok server
  - OpenSolaris
  - OpenTravel Alliance
  - ops center
  - Oracle
  - Oracle Corporation
  - Oracle Database
  - oracle enterprise manager
  - Oracle Exadata
  - Oracle VM Server for SPARC
  - Oracle VM Server for x86
  - san storage
  - series arrays
  - solaris
  - sparc
  - SPARC Enterprise
  - sparc machines
  - stand up comedy
  - storage appliances
  - Sun Microsystems
  - sysadmin
  - T3 processor
  - Technology_Internet
  - virtualization technologies
  - web requests
  - X86-64
  - zones
---
Last night I watched almost the entire 5 hour live webcast announcing Oracle&#8217;s strategies regarding the Sun Microsystems acquisition. As a near-evangelist for Sun and Solaris, I&#8217;m very happy with the deal finally going through and even happier that most of what Oracle said makes sense to me as a customer.

What I liked:

  * The clear commitment to the SPARC roadmap especially the T series. I honestly don&#8217;t know what I would have done if the T series servers disappeared. I&#8217;m very happy that they put raising the clock speed into the roadmap because some applications just can&#8217;t be deployed on these servers.
  * The clear commitment to making waves in Enterprise Storage. NetApp was specifically mentioned and obviously the 7000 series arrays are best suited to compete with the NetApp arrays but I hope they will draw some EMC blood as well. I like the plans for integrating backup capabilities.
  * The plans to integrate really great Solaris tech into Oracle applications like DTrace, and RBAC
  * The plans to offer direct support. Honestly this was one of the most annoying parts of working with Sun was having to work with different support providers in every location.
  * The plans to change the supply chain and ship direct- no more out of stock excuses.
  * The plans to integrate Ops Center with Oracle Enterprise Manager.
  * Larry Ellison&#8217;s stand up comedy
  * And completely unrelated- the flashing disk lights on the Exadata V2 🙂

I didn&#8217;t like:

  * The obvious cut planned for the x64 line of hardware. While they are keeping x64 where convenient (storage appliances, database machines, various other &#8220;clusters&#8221;) it looks like Oracle has no plans for dealing in x64 server business as a server business. I&#8217;m not a big user of the x64 stuff for servers but Sun doesn&#8217;t really offer anything reasonable for entry level anymore except the x64 line. This brings me to my next point-
  * The SPARC roadmap is slightly sucky as in how much processing power do you really want inside a single box.  According to the roadmap, their next plan is to double the amount of cores in a T3 processor so you&#8217;ll have one cpu with 16 cores and 128 threads. Their going to put two in a machine? four?  Here is how I see the servers they have today: 
      * T1000- useless poorly designed server
      * T2000- ok server but a waste of rack space at 2RU
      * T5120- ok server but a waste of rack space considering I could put a T5140 in the same space
      * T5220- worse than the T5120 at 2RU
      * T5140- The best server ever built with exactly the right amount of everything
      * T5240- 2RU again???
      * T5440- I could serve ~8.64 billion web requests per day from one of these but I&#8217;d need a 1.6Gbit uplink and two servers for redundancy = 8RU, or else use 4 T5140 machines, deliver the same performance, and use 4RU?- maybe 5RU including n+1 redundancy.
      * NONEXISTANT &#8211; little SPARC machine for backup/monitoring/insert your SPARC only app that doesn&#8217;t deserve a minimum of 32 threads and 2RU  here.
    
    At some point, you just want more smaller machines for less points of failure. I really have uses for low end SPARC machines and they don&#8217;t make them any more.</li> 
    
      * I don&#8217;t really like the &#8220;server phone home&#8221; idea.
      * No mention of OpenSolaris- I&#8217;m not really a user but I didn&#8217;t like that it wasn&#8217;t mentioned- What does that mean??
      * No mention of Webstack. I really like Sun Webstack as an idea. I&#8217;m not sure what is happening to it now?
      * No mention of how Oracle will be combining the knowledge bases? Sunsolve? Bigadmin? docs.sun.com? forums.sun.com (looks like this already had an Oracle makeover :?)</ul> 
    
    One thing I&#8217;m not sure about is the integration of Sun virtualization technologies into Oracle VM. On one hand it sounds good, on the other hand, I think this was the only part of the presentation where I noticed there were no due dates. Virtualization is super important to me so I really want to know where things stand.
    
    Obviously, it is easy to  get up and say everything will integrate but doing it is much harder. Just getting past the internal politics of this will be a major issue. Now we can only wait and see if Oracle can pull it off.
    
    I used to get upset with &#8220;Oracle people&#8221; for always thinking that Oracle was the solution to every problem. If they pull off this acquisition, I much just become an &#8220;Oracle person&#8221; myself.