---
id: 114
title: 'Tip: scp Resume'
date: 2007-01-31T23:33:00-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2007/01/30-revision.html
permalink: /uncategorized/30-revision.html
---
I left a desktop to download some humongous logs last night using scp and of course the connection was lost in the middle.

I looked for a possible resume feature in scp and found this tip:  
Using rsync to resume partial file transfers: [joen.dk » Tip: scp Resume](http://joen.dk/wordpress/?p=34)

rsync &#8211;partial &#8211;progress -e &#8216;ssh&#8217; &#8230;.