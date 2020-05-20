---
id: 276
title: Sun SPARC T3 Servers
date: 2010-10-16T16:14:44-07:00
author: Yonah Russ
excerpt: |
  Oracle announced their new line of Sun SPARC T3 powered servers at Oracle Openworld 2010. The SPARC T3 processor includes several improvements on T2 and T2+ processors...
  All in all they have packed more T-Series goodness in a smaller package but I'm not making goo-goo eyes yet.
  As a platform for consolidating tens of smaller applications, the thread to RAM ratio is too low making it hard to get 100% utilization out of these servers. With the T3-4 servers loading even more processing power into a single machine, the thread to machine ratio high as well.
layout: post
guid: http://www.yonahruss.com/?p=276
permalink: /architecture/sun-sparc-t3-servers.html
dsq_thread_id:
  - "522877061"
categories:
  - Architecture
tags:
  - 1RU server
  - Computing
  - huge applications
  - Instruction set architectures
  - On Board PCIe x8 v1 Port
  - Open hardware
  - Oracle
  - Oracle Corporation
  - Oracle Database
  - Oracle VM Server for SPARC
  - PCI Express
  - RAM
  - socket systems
  - sparc
  - SPARC T3
  - SPARC T3 processor
  - T2
  - T3 processors
  - T3-1
  - T3-2
  - T3-4
  - T5140
  - T5440
  - Technology_Internet
  - UltraSPARC T1
  - UltraSPARC T2
---
Oracle announced their new line of Sun SPARC T3 powered servers at Oracle Openworld 2010. The SPARC T3 processor includes several improvements on T2 and T2+ processors including:

<table border="1" cellpadding="3">
  <td>
    T2 / T2+
  </td>
  
  <td>
    T3
  </td>
  
  <tr>
    <td>
      65 nm manufacturing process
    </td>
    
    <td>
      40 nm manufacturing process
    </td>
  </tr>
  
  <tr>
    <td>
      4MB L2 Cache
    </td>
    
    <td>
      6MB L2 Cache
    </td>
  </tr>
  
  <tr>
    <td>
      8 Cores (8 threads/core)
    </td>
    
    <td>
      16 Cores (8 threads/core)
    </td>
  </tr>
  
  <tr>
    <td>
      8 Crypto Accelerators (1/core)
    </td>
    
    <td>
      16 Crypto Accelerators (1/core)
    </td>
  </tr>
  
  <tr>
    <td>
      DDR2 FB-DIMMs
    </td>
    
    <td>
      DDR3
    </td>
  </tr>
  
  <tr>
    <td>
      1 On Board PCIe x8 v1 Port
    </td>
    
    <td>
      2 On Board PCIe x8 v2 Ports
    </td>
  </tr>
</table>

It is interesting to note that the T2 processor was only used in single socket systems. The T2+ processor removed the T2&#8217;s on board 10 GbE ports and other components to make room for the SMP glue. With the T3 processors, the 10 GbE ports have returned and the chip has built in glueless support for 4 way servers.

All in all they have packed more T-Series goodness in a smaller package but I&#8217;m not making goo-goo eyes yet.

For one, the smallest T3 based server, the T3-1, has the same number of threads as the T5140 but takes twice as many rack units. Although the T3-1 supports more PCIe cards and more internal hard disks, I would rather have a 1RU server or else have it support twice as much RAM.

The T3-2 server supports 256 threads. Compared to the T5440, it is actually smaller at 3RU and uses less power which sounds like a step in the right direction. Unfortunately, the T3-2 is also light on RAM supporting a maximum of 256GB compared to the T5440&#8217;s 512GB.

In short, The T3 series is a little off course for me at the moment. As a platform for consolidating tens of smaller applications, the thread to RAM ratio is too low making it hard to get 100% utilization out of these servers. With the T3-4 servers loading even more processing power into a single machine, the thread to machine ratio high as well. This is good if you are running a few really huge applications but if you are consolidating many smaller applications, you will not want to put this many eggs in one basket.