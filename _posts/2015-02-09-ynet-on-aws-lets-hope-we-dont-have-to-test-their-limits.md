---
id: 349
title: 'Ynet on AWS. Let&#8217;s hope we don&#8217;t have to test their limits.'
date: 2015-02-09T09:45:20-07:00
author: Yonah Russ
layout: post
guid: http://www.yonahruss.com/?p=349
permalink: /architecture/cloud/ynet-on-aws-lets-hope-we-dont-have-to-test-their-limits.html
categories:
  - Availability
  - Cloud
tags:
  - Amazon
  - Amazon Elastic Compute Cloud
  - Amazon Web Services
  - Cascading failure
  - Cloud computing
  - Cloud infrastructure
  - EBS
  - EC2
  - Israel
  - load balancing
  - Web services
---
<img class="aligncenter size-full wp-image-350" src="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/tightrope.jpg" alt="tightrope" width="698" height="400" srcset="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/tightrope.jpg 698w, http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/tightrope-300x172.jpg 300w" sizes="(max-width: 698px) 100vw, 698px" />

In Israel, more than in most places, no news is good news. Ynet, one of the largest news sites in Israel, recently posted a case study (at the bottom of this article) on handling large loads by moving their notification services to AWS.

> &#8220;We used EC2, Elastic Load Balancers, and EBS&#8230; Us as an enterprise, we need something stable&#8230;&#8221;

They are contradicting themselves in my opinion. EBS and Elastic Load Balancers (ELB) are the two AWS services which fail the most and fail hardest with multiple downtimes spanning multiple days each.

## EBS: Conceptually flawed, prone to cascading failures

EBS, a virtual block storage service, is conceptually flawed and prone to severe cascading failures. In recent years, Amazon has improved reliability somewhat, mainly by providing such a low level of service on standard EBS, that customers are default to paying extra for reserved IOPS and SSD backed EBS volumes.

Many cloud providers avoid the <a href="https://www.joyent.com/blog/network-storage-in-the-cloud-delicious-but-deadly/" target="_blank" rel="nofollow">problematic nature of virtual block storage</a> entirely, preferring compute nodes based on local, direct attached storage.

## ELB: Too slow to adapt, silently drops your traffic

In my experience, ELBs are too slow to adapt to spikes in traffic. About a year ago, I was called to investigate availability issues with one of our advertising services. The problems were intermittent and extremely hard to pin down. Luckily, as a B2B service, our partners noticed the problems. Our customers would have happily ignored the blank advertising space.

Suspecting some sort of capacity problem, I ran some synthetic load tests and compared the results with logs on our servers. Multiple iterations of these tests with and without ELB in the path confirmed a gruesome and silent loss of 40% of our requests when traffic via Elastic Load Balancers grew suddenly.

The Elastic Load Balancers gave us no indication that they were dropping requests and, although they would theoretically support the load once Amazon&#8217;s algorithms picked up on the new traffic, they just didn&#8217;t scale up fast enough. We wasted tons of money in bought media that couldn&#8217;t show our ads.

Amazon will <a href="https://aws.amazon.com/articles/1636185810492479#pre-warming" target="_blank" rel="nofollow">prepare your ELBs for more traffic</a> if you give them two weeks notice and they&#8217;re in a good mood but who has the luxury of knowing when a spike in traffic will come?

## Recommendations

I recommend staying away from EC2, EBS, and ELB if you care about performance and availability. There are better, more reliable providers like <a href="https://www.joyent.com/" target="_blank" rel="nofollow">Joyent</a>. <a href="http://www.rackspace.com/" target="_blank" rel="nofollow">Rackspace</a> without using their cloud block storage (basically the same as EBS with the same flaws) would be my second choice.

If you must use EC2, try to use load balancing AMIs from companies like <a href="https://aws.amazon.com/marketplace/seller-profile?id=325de86e-df57-4cfa-907d-9450285549c5" target="_blank" rel="nofollow">Riverbed </a>or <a href="https://aws.amazon.com/marketplace/seller-profile?id=74d946f0-fa54-4d9f-99e8-ff3bd8eb2745" target="_blank" rel="nofollow">F5</a> instead of ELB.

If you must use ELB, make sure you run synthetic <a href="https://loader.io/" target="_blank" rel="nofollow">load</a> <a href="http://blazemeter.com/" target="_blank" rel="nofollow">tests</a> at random intervals and make sure that Amazon isn&#8217;t dropping your traffic.

## Conclusion

In conclusion, let us hope that we have no reasons to test the limits of Ynet&#8217;s new services, and if we do, may it only be good news.