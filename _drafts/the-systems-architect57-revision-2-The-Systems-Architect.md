---
id: 148
title: The Systems Architect
date: 2009-11-08T13:52:42-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2009/11/57-revision-2.html
permalink: /tips/57-revision-2.html
---
What is a Systems Architect?

Systems Architects have years of experience in the various parts of the systems they work with. Most probably, they specialize in a specific area area of expertise where they began their careers, but have since expanded their knowledge by learning from their colleagues and from life&#8217;s lessons. To get to their position, they have proven their ability to analyze and understand the needs and constraints of the business they work in. They are responsible for deciding what technologies will provide the best solutions for a business, how to integrate them with existing systems, and how to retire them when they are obsolete.

There are several flavors of Systems Architects.

  1. <span style="font-weight: bold;">The Bang-For-Your-Buck Architect</span>&#8211; They can cut your OP-EX by 90% and consolidate all your servers on to two desktops running VMWare if you give them enough time and rewrite your application in assembler. They live and get fired by the [Project Management Triangle](http://www.yonahruss.com/exit.php?url=en.wikipedia.org/wiki/Project_triangle#Project_Management_Triangle), always aiming for High Quality, Cheap, but Slower than molasses.
  2. <span style="font-weight: bold;">The Unbreakable Architect</span>&#8211; Their systems do not know the meaning of the word Downtime but come at the price of top of the line co-location, ISP level network equipment, 2n+1 hardware, months of code freezes, release testing cycles, staging and performance test runs, security scans and rejects.
  3. <span style="font-weight: bold;">The Performance Architect</span>&#8211; Optimzation is the name of the game. Your site will be in the customer&#8217;s browser before they can blink (remember no flash, no heavy images, no javascript and no calls to google analytics). Your database will not have time to read from the disk before it answers your queries (you didn&#8217;t really invest in multiple Oracle RAC nodes to access the same data at the same time? everyone knows that just slows the database down!)
  4. <span style="font-weight: bold;">The All-Of-The-Above Architect</span>&#8211; Everything is important in the right balance. Consolidation and cutting costs is a factor but so is ease of implementation. The system should be flexible enough to handle new requirements quickly but getting something done quickly is not an excuse for poor implementation. The system must be robust but not every risk is worth mitigating. Cost/Performance is quantitative. It should be measured and improved if necessary in the most cost effective manner. The work of finding balance in a system never ends but the most important thing is to always move towards the end goal.

If you know a Systems Architect like those described above, please have pity on him. He is really trying to do what he thinks is best and he really does (probably) have a lot of experience.

Next time- &#8220;The Software Architect&#8221;