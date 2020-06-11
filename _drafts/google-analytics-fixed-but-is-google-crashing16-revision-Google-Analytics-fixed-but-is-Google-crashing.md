---
id: 119
title: Google Analytics fixed but is Google crashing?
date: 2006-05-05T01:04:00-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2006/05/16-revision.html
permalink: /uncategorized/16-revision.html
---
Google has finally added Israel to the list of Countries in the sign up process which is good news.  
On the other hand, they got the timezone wrong (Israel is in DST right now and uses GMT+3 till about October) and that&#8217;s after spending over a week fixing it.

What&#8217;s going on inside Google? Why did it take so long? Why wasn&#8217;t Israel on the list to begin with?  
I recieved no explanation from Google but my guess is that they must of had a bug in the code generating the form fields and the javascript behind them. Look at the following code sample:

    CC["ID"] = new Array("220|(GMT+07:00) Jakarta","234|(GMT+08:00) Makassar","221|(GMT+09:00) Jayapura");<br />CC["IR"] = new Array("257|(GMT+03:30) Tehran");<br />CC["IQ"] = new Array("198|(GMT+03:00) Baghdad");<br />CC["IE"] = new Array("300|(GMT+00:00) Greenwich Mean Time");<br />CC["IL"] = new Array("222|(GMT+02:00) Jerusalem");

Before the fix, Jerusalem time was present in the javascript but it was ORed to something else like the first line above.

That is still no excuse for such a system going live. Google&#8217;s quality control should have stepped in.

On the other hand it points to a growing list of technical difficulties within Google.

  1. Since Google&#8217;s last update to their algorithms, they&#8217;ve been returning pages from sites of mine that haven&#8217;t been online in years.
  2. For over a week I&#8217;ve been experiencing problems with Gmail timing out. 
  3. Blogger is less than responsive as always.
  4. Analytics, in the day that I&#8217;ve been using it, has often claimed to be under maintenance one second and fine the next- I guess maintenance means &#8220;I&#8217;m a tired server, leave me alone please.&#8221;

The Register reports that Google is choking on web spam: [http://www.theregister.co.uk/2006/05/04/google\_bigdaddy\_chaos/](http://www.theregister.co.uk/2006/05/04/google_bigdaddy_chaos/)

> Webmasters now report sites not being crawled for weeks, with Google SERPS (search engine results pages) returning old pages, and failing to return results for phrases that used to bear fruitful results.
> 
> &#8220;Some sites have lost 99 per cent of their indexed pages,&#8221; reports one member of the _Webmaster World_ forum. &#8220;Many cache dates go back to 2004 January.&#8221; Others report long-extinct pages showing up as &#8220;Supplemental Results.&#8221; 
> 
> But the new algorithms may not be solely to blame. Google&#8217;s chief executive Eric Schmidt has hinted at another reason for the recent chaos. In Google&#8217;s earnings conference call last month, Schmidt was frank about the extent of the problem.
> 
> &#8220;Those machines are full,&#8221; he <a href="http://www.iht.com/articles/2006/04/21/business/GOOGLE.php" target="_blank">said</a>. &#8220;We have a huge machine crisis.&#8221;

While [here](http://www.cincomsmalltalk.com/blog/blogView?showComments=true&entry=3324214398) they attempt to save face for Google by putting Schmidt&#8217;s comment in context, it&#8217;s clear that Google has been having technical problems.

> Google continued to make substantial capital investments, mainly in computer servers, networking equipment and its data centers. It spent $345 million on such items in the first quarter, more than double the level of last year. Yahoo, its closest rival, spent $142 million on capital expenses in the first quarter.
> 
> Referring to the sheer volume of Web site information, video and e-mail that Google&#8217;s servers hold, Schmidt said: &#8220;Those machines are full. We have a huge machine crisis.&#8221;
> 
> Jordan Rohan of RBC Capital Markets called Google&#8217;s capital spending &#8220;unfathomably high,&#8221; noting that it spent the same percentage of its revenue on equipment as a wire-line phone company.

I don&#8217;t see how the context makes things any better. The bottom line remains that Google needed a heck of a lot more hardware than it had and who knows if they bought everything they needed. Those are only the first quarter figures- I would imagine it could take a whole quarter to deploy $345 million dollars of equipment. I wonder what they will spend next quarter?