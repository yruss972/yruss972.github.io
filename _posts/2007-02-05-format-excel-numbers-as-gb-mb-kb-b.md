---
id: 33
title: Format Excel numbers as GB, MB, KB, B
date: 2007-02-05T01:14:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2007/02/format-excel-numbers-as-gb-mb-kb-b.html
permalink: /tips/format-excel-numbers-as-gb-mb-kb-b.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2007/02/format-excel-numbers-as-gb-mb-kb-b.html
dsq_thread_id:
  - "522877822"
categories:
  - Tips
tags:
  - excel
  - gigabytes megabytes
  - gt power
  - megabytes kilobytes
  - Technology_Internet
  - Units of information
---
Here is a great tip for formatting numbers in Excel as Gigabytes, Megabytes, Kilobytes, Bytes, etc.  
Use the following formula:

    =IF((A1>=POWER(2,30)),
         TEXT((A1/POWER(2,30)),"##0.00\G"),
         IF((A1>=POWER(2,20)),
               TEXT((A1/POWER(2,20)),"##0.00\M"),
               IF((A1>=1024),
                   TEXT((A1/1024),"##0.00\K"),
                   TEXT((A1),"##0.00\B")
                   )
             )
        )
    

The same formula will work for Gigabits, Megabits, etc. assuming you start with bits instead of bytes. If you want to convert from bits to bytes in the process, use this formula:

    =IF((A1>=POWER(2,33)),
         TEXT((A1/POWER(2,33)),"##0.00\G"),
         IF((A1>=POWER(2,23)),
               TEXT((A1/POWER(2,23)),"##0.00\M"),
               IF((A1>=8192),
                   TEXT((A1/8192),"##0.00\K"),
                   TEXT((A1/8),"##0.00\B")
                   )
             )
        )