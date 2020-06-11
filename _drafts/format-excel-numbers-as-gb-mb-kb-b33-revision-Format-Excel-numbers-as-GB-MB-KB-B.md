---
id: 112
title: Format Excel numbers as GB, MB, KB, B
date: 2007-02-05T01:14:00-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2007/02/33-revision.html
permalink: /uncategorized/33-revision.html
---
Here is a great tip for formatting numbers in Excel as Gigabytes, Megabytes, Kilobytes, Bytes, etc.  
Use the following formula:

    =IF((A1>=POWER(2,30)),<br />     TEXT((A1/POWER(2,30)),"##0.00\G"),<br />     IF((A1>=POWER(2,20)),<br />           TEXT((A1/POWER(2,20)),"##0.00\M"),<br />           IF((A1>=1024),<br />               TEXT((A1/1024),"##0.00\K"),<br />               TEXT((A1),"##0.00\B")<br />               )<br />         )<br />    )<br />

The same formula will work for Gigabits, Megabits, etc. assuming you start with bits instead of bytes. If you want to convert from bits to bytes in the process, use this formula:

    =IF((A1>=POWER(2,33)),<br />     TEXT((A1/POWER(2,33)),"##0.00\G"),<br />     IF((A1>=POWER(2,23)),<br />           TEXT((A1/POWER(2,23)),"##0.00\M"),<br />           IF((A1>=8192),<br />               TEXT((A1/8192),"##0.00\K"),<br />               TEXT((A1/8),"##0.00\B")<br />               )<br />         )<br />    )<br />