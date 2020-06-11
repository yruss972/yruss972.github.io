---
id: 129
title: 'When 99.999% Isn&#8217;t Good Enough'
date: 2009-11-09T12:18:56-07:00
author: Yonah Russ
excerpt: |
  When discussing availability of a service, it is common to hear the term "Five Nines" referring to a service being available 99.999% of the time but "Five Nines" are relative.
  ...
  In reality, none of those calculations are relevant because no one cares if a service is unavailable for 10 hours, as long as they aren't trying to use it. On the other hand, if you're handling 50,000 transactions per second, 6.05 seconds of unavailability could cost you 302,500 transactions and no one cares if you met your SLA.
layout: revision
guid: http://yonahruss.com/wordpress/2009/11/61-autosave.html
permalink: /tips/61-autosave.html
---
When discussing [availability](http://www.yonahruss.com/exit.php?url=en.wikipedia.org/wiki/High_availability) of a service, it is common to hear the term &#8220;Five Nines&#8221; referring to a service being available 99.999% of the time but &#8220;Five Nines&#8221; are relative. If your time frame is a week, then your service can be unavailable for 6.05 seconds whereas a time frame of a year, allows for a very respectable 5.26 minutes.

In reality, none of those calculations are relevant because no one cares if a service is unavailable for 10 hours, as long as they aren&#8217;t trying to use it. On the other hand, if you&#8217;re handling 50,000 transactions per second, 6.05 seconds of unavailability could cost you 302,500 transactions and no one cares if you met your SLA.

This problem is one I&#8217;ve come up against a number of times in the past and recently even more and the issue is orders of magnitude in IT. The larger the volume of business you handle, the less relevant the Five Nines become.

Google became famous years ago for its novel approach to hardware availability. They were using servers and disks on such a scale that they could no longer prevent the failures and they decided not to even try. Instead, they planned to sustain lots of failures and made a business of knowing when to expect problems and where. As much as we would like to be able to take Google&#8217;s approach to things, I think most of our IT budgets aren&#8217;t up for it.

Another good example is EMC<sup>2</sup> who boast 99.999% availability for their Clariion line of storage systems. I want to start by saying that I use EMC storage and I&#8217;m happy with them. Regardless, their claim of 99.999% availability doesn&#8217;t give me any comfort for the following reasons.

According to a Whitepaper from 2007 (maybe they have changed things since then) EMC has a team which calculates availability for every Clariion in the field on a weekly basis. Assuming there were 2000 Clariion systems in the field on a given week(the example given in the whitepaper), and across all of them was 1.5 hours of downtime, then:

    2000 systems x 7 days x 24 hours   =  336,000 total hours of runtime
    336,000 hours - 1.5 hours downtime =  335,998.5 hours of uptime
    335,998.5 / 336,000                =  99.9996% uptime

That is great, at least that is what EMC wants you to think. I look at this and understand something totally different. According to [this guy](http://www.yonahruss.com/exit.php?url=storagezilla.typepad.com/storagezilla/2009/01/index.html), as of the beginning of 2009 there were 300,000 Clariion&#8217;s sold- not 2000. That is two orders of magnitude different meaning:

    300,000 systems x 7 days x 24 hours   =  50,400,000 total hours of runtime
    336,000 hours - 504 hours downtime    =  50,399,496 hours of uptime
    50,399,496 / 50,400,000               =  99.999% uptime

Granted, that is a lot of uptime but 504 hours of downtime is still 21 full days of downtime for someone. If it were possible for 21 full days of downtime to fit in one week, they could all be yours and EMC would still be able to claim 99.999% availability according to their calculations. By the same token, 3 EMC customers each week could theoretically have no availability the entire week and one of those customers could be me.

Since storage failures can cause soo many complications, I figure it is much more likely that EMC downtime comes in days as opposed to minutes or hours. Either way, Five Nines is lost in the scale of things in this case as well.

Content Delivery Networks provide another availability vs scale problem. Akamai announced record breaking amounts of traffic on their network in January 2009. They passed 2 terabits and 12,000,000 requests per second. (I don&#8217;t use Akamai but I think it is amazing that they delivered over 2 terabits/second of traffic). With that level of traffic, even if Akamai would provide a 99.999% availability SLA, they could have had 120 failed requests per second, 7200 failed requests per minute, etc.

Sometimes complaints relating to our CDN cross my desk and while I have no idea how much traffic our CDN handles world wide, I know that we can easily send it 20,000,000 requests per day. Assuming 99.999% availability, I expect (learning from Google) to have 200 failed requests per day. Knowing IT as I do, I also expect that all 200 failed requests will be in the same country -probably an issue with one of their cache servers which due to GTM will primarily affect people directed to that server, etc. Unfortunately, the issue of scale is lost on our partners who didn&#8217;t get their content.

Availability is not the only case where scale is forgotten. I was recently asked to help debug the performance of an application server which could handle a large amount of requests per second when queried directly but only handled 80% of the requests per second when sitting behind a load balancer.

Of course we started by trying to find a reason why the load balancer would be causing a 20% performance hit. After deep investigation the answer I found (not necessarily the correct answer) was that all the load balancing configurations were correct and on average having the load balancer in the path added 1 millisecond to the response time of each request. Unfortunately the response time without the load balancer was an average of 4 milliseconds, so the additional 1 millisecond reduced the overal performance by 20%.

In short, everything is relative and 99.999% isn&#8217;t good enough.