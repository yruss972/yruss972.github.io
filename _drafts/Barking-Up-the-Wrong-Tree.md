---
id: 10
title: Barking Up the Wrong Tree
date: 2009-08-02T15:41:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/?p=10
permalink: /?p=10
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /feeds/2928092265602832354/posts/default/7167553003741939208
categories:
  - Uncategorized
---
Often, the software and infrastructure components sit in completely separate domains of responsibility within an organization. This is prevalent when the software requirements from the infrastructure are minimal; the Software Architect and System Architect can work independently.

When the software becomes more complex, this is no longer the case. The lines between software and infrastructure are blurred. A simple example of this is the [Multi-Tier Architecture](http://www.yonahruss.com/exit.php?url=en.wikipedia.org/wiki/Multitier_architecture)  
used by many applications. Another classic example is found in any system which relies on a high performance database. High performance databases rely heavily on extremely fast Disk IO, large amounts of memory and network bandwidth. They can sometimes require little concurrency but powerful processors, and sometimes the opposite: high concurrency and less powerful processors.