---
id: 174
title: 'RAID 10 vs RAID 5: Performance, Cost, Space, and HA'
date: 2009-11-08T13:53:06-07:00
author: Yonah Russ
layout: revision
guid: http://www.yonahruss.com/2009/11/52-revision-3.html
permalink: /tips/52-revision-3.html
---
DISCLAIMER: I am not a SAN storage expert but I have spent a lot of time looking into SAN storage systems from the business side and I thought I&#8217;d share some of my conclusions.

It seems that the proverbial question is how to balance the performance, cost, usable space, and availability of a storage solution. Any DBA will ask you to give him RAID 10 on small fast disks. Anyone paying the bills will ask &#8220;Why can&#8217;t I use half the disks I bought?&#8221;

I took a couple hours with your friendly neighborhood spreadsheet and did the math. I base my calculations on EMC Clariion storage and tried to follow the EMC best practices guide as much as possible.

According to the best practices, I started my calculations based on a necessary performance level consisting of total IOPS, read percentage, and write percentage.  
Then, using the following formulas, I calculate the actual disk IOPS required to provide the requested performance:

  * RAID 5 (4+1 Groups)

> <pre>Disk IOPS = (Read % * Required IOPS) + (Write % * RAID 5 write penalty * Required IOPS)</pre>

  * RAID 10

> <pre>Disk IOPS = (Read % * Required IOPS) + (Write % * RAID 10 write penalty * Required IOPS)</pre>

The RAID 5 write penalty in a 4+1 RAID group is 4 while the RAID 10 write penalty is 2.  
Before you even put this in a spreadsheet you know what it will tell you-

  * In a 100% Read Only environment RAID 5 and RAID 10 will give the same performance. RAID 5 may use less disks to do it but not necessarily.
  * In a 100% Write Only environment, RAID 5 will require twice as many disk IOPS and almost twice the number of disks.
  * Anywhere in between those two extremes, the more writes required, the less number of RAID 10 disks you will need to achieve the performance.

If we stop there, it doesn&#8217;t seem like there is any point in using RAID 5 since even in the best case scenario, there is only a partial chance that we will use less disks. That is where the cost and space effectiveness issues come in.

  * Space Effective Storage Allocation

> If I want 2000 IOPS, 100% Read Only, I can do that using 15 x 146GB 15k RPM disks in RAID 5 or in RAID 10. In RAID 5 I will get ~1.5TB net space while in RAID 10 I will get ~1TB.

  * Cost Effective Storage Allocation

> So far, we have compared different RAID types using the same size and speed disks and we saw that theoretically we can use less disks to reach the same performance but at the expense of usable disk space.
> 
> If we use bigger disks for the RAID 10, does it make up for the lost space? What effect does using RAID 10 with fewer large disks as opposed to RAID 5 with lots of smaller disks have on the cost of my solution?

That brings us back to the spreadsheet. Using the required disk IOPS we can figure out the required number of physical disks of each type. For the sake of comparison I use the following information which I found on the Internet (your mileage may vary):

  * 146GB 4GbFC 15k RPM, 140 IOPS, $1256
  * 300GB 4GbFC 10k RPM, 120 IOPS, $1348
  * 1TB 4Gb SATA II 7.2k RPM, 80 IOPS, $2088

For each of these I calculate the minimum number of physical disks required for to reach the required IOPS with the required read/write profile for both RAID 10 and RAID 5. Then I figure in the RAID group sizes and calculated the usable disk space.

Using the prices above, I calculate the price per TB of disk space in each RAID configuration and find:

  * 146GB, RAID 5 (4+1): $11.91K/TB
  * 300GB, RAID 5 (4+1): $6.35K/TB
  * 1TB, RAID 5 (4+1): $2.87K/TB
  * 146GB, RAID 10 (4+1): $19.01K/TB
  * 300GB, RAID 10: $10.15K/TB
  * 1TB, RAID 10: $4.59K/TB

What is really interesting here is how close the 300GB RAID 10 is to the 146GB RAID 5! Is this a coincidence?

Looking at the IOPS/TB relationship and $K/IOPS, we find that the ratios are dependant on the read/write profile of the required IOPS. Given the similar Price/TB of 300GB RAID 10 and 146GB RAID 5, I look there for a price/performance/disk space sweet spot.

The following table shows the difference between 146GB RAID 5 IOPS/TB and 300GB RAID 10 IOPS/TB.  
Each column represents a different Read percentage (the Write percentage is the inverse).  
Negative numbers mean that for this Read percentage and IOPS requirement, RAID 10 gives more IOPS/TB of disk. Positive numbers mean that RAID 5 gives better IOPS/TB.



What you see from this is that for any read workload under 70%, you will get more IOPS/TB from 300GB 10k RPM disks using RAID 10 than you will with RAID 5 on 146GB 15k RPM disks.  
Even if you hit 80%, RAID 5 will gain less than 100 IOPS over the RAID 10 configuration and you are still better off paying less for your disks- let the cache do it&#8217;s job. Combine all this with our previous conclusion &#8211; that the 300GB RAID 10 configuration is ~$1.75K less expensive per TB and I say you have a winner.