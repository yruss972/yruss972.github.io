---
id: 176
title: Format Excel numbers as GB, MB, KB, B
date: 2009-11-23T15:38:11-07:00
author: Yonah Russ
layout: revision
guid: http://www.yonahruss.com/2009/11/33-autosave.html
permalink: /tips/33-autosave.html
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