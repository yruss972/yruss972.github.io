---
id: 9
title: The Software Architect
date: 2009-11-10T12:12:17-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/?p=9
permalink: /?p=9
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /feeds/2928092265602832354/posts/default/5921609174083463763
categories:
  - Uncategorized
tags:
  - algorithms and data structures
  - Buck Architect-
  - C
  - IO
  - performance databases
  - slower than molasses
  - Software
  - software architect
  - System
  - system architects
---
Please don&#8217;t hire a Programmer as your Software Architect!

How do you know the difference, you ask. Both probably have extensive knowledge in programming. They have both probably been trained in some sort of [Imperative Programming](http://www.yonahruss.com/exit.php?url=en.wikipedia.org/wiki/Imperative_programming) (C, C++, Java, Python, etc.) They have both studied Design Patterns, Algorithms, and Data Structures.

The Architect, however, understands what is outside the realm of programming.  
He understands the <span style="font-style: italic;">business</span>. He can take feature requests from customers and turn them into technical requirements.  
He also understands infrastructure. He knows the difference between a server and cluster of servers. He knows the difference between disk access and memory access.

Often, the software and infrastructure components sit in completely separate domains of responsibility within an organization. This is prevalent when the software requirements from the infrastructure are minimal; the Software Architect and System Architect can work independently.

When the software becomes more complex, this is no longer the case. The lines between software and infrastructure are blurred. A simple example of this is the [Multi-Tier Architecture](http://www.yonahruss.com/exit.php?url=en.wikipedia.org/wiki/Multitier_architecture)  
used by many applications. Another classic example is found in any system which relies on a high performance database. High performance databases rely heavily on extremely fast Disk IO, large amounts of memory and network bandwidth. They can sometimes require little concurrency but powerful processors, and sometimes the opposite: high concurrency and less powerful processors.

There are several flavors of System Architects.

  1. <span style="font-weight: bold;">The Bang-For-Your-Buck Architect</span>&#8211; They can cut your OP-EX by 90% and consolidate all your servers on to two desktops running VMWare if you give them enough time and rewrite your application in assembler. They live and get fired by the [Project Management Triangle](http://www.yonahruss.com/exit.php?url=en.wikipedia.org/wiki/Project_triangle#Project_Management_Triangle), always aiming for High Quality, Cheap, but Slower than molasses.
  2. <span style="font-weight: bold;">The Unbreakable Architect</span>&#8211; Their systems do not know the meaning of the word Downtime but come at the price of top of the line co-location, ISP level network equipment, 2n+1 hardware, months of code freezes, release testing cycles, staging and performance test runs, security scans and rejects.
  3. <span style="font-weight: bold;">The Performance Architect</span>&#8211; Optimzation is the name of the game. Your site will be in the customer&#8217;s browser before they can blink (remember no flash, no heavy images, no javascript and no calls to google analytics). Your database will not have time to read from the disk before it answers your queries (you didn&#8217;t really invest in multiple Oracle RAC nodes to access the same data at the same time? everyone knows that just slows the database down!)
  4. <span style="font-weight: bold;">The All-Of-The-Above Architect</span>&#8211; Everything is important in the right balance. Consolidation and cutting costs is a factor but so is ease of implementation. The system should be flexible enough to handle new requirements quickly but getting something done quickly is not an excuse for poor implementation. The system must be robust but not every risk is worth mitigating. Cost/Performance is quantitative. It should be measured and improved if necessary in the most cost effective manner. The work of finding balance in a system never ends but the most important thing is to always move towards the end goal.

If you know a System Architect like those described above, please have pity on him. He is really trying to do what he thinks is best and he really does (probably) have a lot of experience.

Next time- &#8220;The Developer Doesn&#8217;t Mean to be Evil&#8221;