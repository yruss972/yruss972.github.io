---
id: 330
title: Wrangling Elephants in the Cloud
date: 2015-01-11T09:09:29-07:00
author: Yonah Russ
layout: post
guid: http://www.yonahruss.com/?p=330
permalink: /architecture/wrangling-elephants-in-the-cloud.html
categories:
  - Architecture
  - Availability
  - Cloud
  - Tips
tags:
  - Amazon
  - Amazon Web Services
  - Amazon.com
  - auto-scaling solutions
  - Brendan Gregg
  - Cloud computing
  - Cloud computing issues
  - Cloud infrastructure
  - configuration management
  - DNS
  - Netflix
  - system monitoring solution
  - Technology_Internet
---
<img class="aligncenter size-full wp-image-331" src="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/elephant.jpg" alt="elephant" width="600" height="343" srcset="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/elephant.jpg 600w, http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/elephant-300x172.jpg 300w" sizes="(max-width: 600px) 100vw, 600px" />

You know the elephant in the room, the one no one wants to talk about. Well it turns out there was a whole herd of them hiding in my cloud. There&#8217;s a herd of them hiding in your cloud too. I&#8217;m sure of it. Here is my story and how I learned to wrangle the elephants in the cloud.

Like many of you, my boss walked into my office about three years ago and said &#8220;We need to move everything to the cloud.&#8221; At the time, I wasn&#8217;t convinced that moving to the cloud had technical merit. The business, on the other hand, had decided that, for whatever reason, it was _absolutely_ necessary.

As I began planning the move, selecting a cloud provider, picking tools with which to manage the deployment, I knew that I wasn&#8217;t going to be able to provide the same quality of service in a cloud as I had in our server farm. There were too many unknowns.

The cloud providers don&#8217;t like to give too many details on their setups nor do they like to provide many meaningful SLAs. I have very little idea what hardware I&#8217;m running. I have almost no idea how it&#8217;s connected. How many disks I&#8217;m running on? What RAID configuration? How many IOPS can I count on? Is a disk failing? Is it being replaced? What will happen if the power supply blows? Do I have redundant network connections?

Whatever it was that made the business decide to move, it trumped all these unknowns. In the beginning, I focused on getting what we had from one place to the other, following whichever tried and true best practices were still relevant.

Since then, I&#8217;ve come up with these guiding principles for working around the unknowns in the cloud.

  * Beginners: 
      * Develop in the cloud
      * Develop for failure
      * Automate deployment to the cloud
      * Distribute deployments across regions
  * Advanced: 
      * Monitor everything
      * Use multiple providers
      * Mix and match private cloud

## Wrangling elephants for beginners:

### Develop in the cloud.

Developers invariably want to work locally. It&#8217;s more comfortable. It&#8217;s faster. It&#8217;s why you bought them a crazy expensive MacBook Pro. It is also nothing like production and nothing developed that way ever really works the same in real life.

If you want to run with the IOPS limitations of standard Amazon EBS or you want to rely on Amazon ELBs to distribute traffic under <a href="http://blog.mattheworiordan.com/post/24620577877/part-2-how-elastic-are-amazon-elastic-load" target="_blank" rel="nofollow">sudden</a> <a href="http://harish11g.blogspot.co.il/2012/07/aws-elastic-load-balancing-elb-amazon.html" target="_blank" rel="nofollow">load</a>, you need to have those limitations in development as well. I&#8217;ve seen developers cry when their MongoDB deployed to EBS and I&#8217;ve seen ELBs disappear 40% of a huge media campaign.

### Develop for failure.

Cloud providers will fail. It is cheaper for them to fail and in the worst case, credit your account for some machine hours, than it is for them to buy high quality hardware and setup highly available networks. In many cases, the failure is not even a complete and total failure (that would be too easy). Instead, it could just be some <a href="http://www.networkworld.com/article/2160902/cloud-computing/amazon-ebs-failure-brings-down-reddit--imgur--others.html" target="_blank" rel="nofollow">incredibly high response times</a> which your application may not know how to deal with.

You need to develop your application with these possibilities in mind. <a href="http://techblog.netflix.com/2012/07/chaos-monkey-released-into-wild.html" target="_blank" rel="nofollow">Chaos Monkey</a> by Netflix is a classic, if not over-achieving example.

### Automate deployment to the cloud.

I&#8217;m not even talking about more complicated, possibly over complicated, auto-scaling solutions. I&#8217;m talking about when it&#8217;s 3am and your customers are switching over to your competitors. Your cloud provider just lost a rack of machines including half of your service. You need to redeploy those machines ASAP, possibly to a completely different data center.

If you&#8217;ve automated your deployments and there aren&#8217;t any other hiccups, it will hopefully take less than 30 minutes to get back up. If not, well, it will take what it takes. There are many other advantages to automating your deployments but this is the one that will let you sleep at night.

### Distribute deployments across regions.

A pet peeve of mine is the mess that Amazon has made with their &#8220;availability zones.&#8221; While the concept is a very easy to implement solution (from Amazon&#8217;s point of view) to the logistical problems involved in running a cloud service, it is a constantly overlooked source of unreliability for beginners choosing Amazon AWS. Even running a multi-availability zone deployment in Amazon only marginally increases reliability whereas deploying to multiple regions can be much more beneficial with a similar amount of complexity.

Whether you use Amazon or another provider, it is best to build your service from the ground up to run in multiple regions, even only in an active/passive capacity. Aside from the standard benefits of a distributed deployment (mitigation of DDOS attacks and uplink provider issues, lower latency to customers, disaster recovery, etc.), running in multiple regions will protect you against regional problems caused by <a href="https://aws.amazon.com/message/67457/" target="_blank" rel="nofollow">hardware failure</a>, <a href="https://aws.amazon.com/message/65648/" target="_blank" rel="nofollow">regional maintenance</a>, or <a href="https://aws.amazon.com/message/680587/" target="_blank" rel="nofollow">human error</a>.

## Advanced elephant wrangling:

The four principles before this are really about being prepared for the worst. If you&#8217;re prepared for the worst, then you&#8217;ve managed 80% of the problem. You may be wasting resources or you may be susceptible to provider level failures, but your services should be up all of the time.

### Monitor Everything.

It is very hard to get reliable information about system resource usage in a cloud. It really isn&#8217;t in the cloud provider&#8217;s interest to give you that information, after all, they are making money by overbooking resources on their hardware. No, you shouldn&#8217;t rely on Amazon to monitor your Amazon performance, at least not entirely.

Even when they give you system metrics, it might not be the information you need to solve your problem. I highly recommend reading the book <a href="http://www.amazon.com/Systems-Performance-Enterprise-Brendan-Gregg/dp/0133390098" target="_blank" rel="nofollow">Systems Performance &#8211; Enterprise and the Cloud</a> by <a href="http://www.brendangregg.com/" target="_blank" rel="nofollow">Brendan Gregg</a>.

Some clouds are <a href="https://www.joyent.com/" target="_blank" rel="nofollow">better</a> than others at providing system metrics. If you can choose them, great! Otherwise, you need to start finding other strategies for monitoring your systems. It could be to monitor your services higher up in the stack by adding more metric points to your code. It could be to audit your request logs. It could be to install an APM agent.

Aside from monitoring your services, you need to monitor your providers. Make sure they are doing their jobs. Trust me that some times they aren&#8217;t.

I highly recommend monitoring your services from <a href="http://newrelic.com/" target="_blank" rel="nofollow">multiple</a> <a href="http://circonus.com/" target="_blank" rel="nofollow">points</a> of <a href="http://keynote.com/" target="_blank" rel="nofollow">view </a>so you can corroborate the data from multiple observers. This happens to fit in well with the next principle.

### Use multiple providers.

There is no way around it. Using one provider for any third party service is putting all your eggs in one basket. You should use multiple providers for everything in your critical path, especially the following four:

  * DNS
  * Cloud
  * CDN
  * Monitoring

Regarding DNS, there are some great providers out there. CloudFlare is a great option for the budget conscious. Route53 is not free but not expensive. DNSMadeEasy is a little bit pricier but will give you some more advanced DNS features. Some of the nastiest <a href="https://cloudharmony.com/status-1year-of-dns-group-by-regions-and-provider" target="_blank" rel="nofollow">downtimes</a> in the past year were due to DNS provider

Regarding Cloud, using multiple providers requires very good automation and configuration management. If you can find multiple providers which run the same underlying platform (for example, Joyent licenses out their cloud platform to various other public cloud vendors), then you can save some work. In any case, using multiple cloud providers can save you from some <a href="https://cloudharmony.com/status-1year-of-compute-group-by-regions-and-provider" target="_blank" rel="nofollow">downtime</a>, bad cloud maintenance or <a href="https://www.google.com/?q=cloud+provider+closing+doors#q=cloud+provider+closing+doors" target="_blank" rel="nofollow">worse</a>.

CDNs also have their <a href="https://cloudharmony.com/status-1year-of-cdn-group-by-regions-and-provider" target="_blank" rel="nofollow">ups and downs</a>. The Internet is a fluid space and one CDN may be faster one day and slower the next. A good <a href="http://www.cedexis.com/" target="_blank" rel="nofollow">Multi-CDN</a> solution will save you from the bad days, and make every day a little better at the same time.

Monitoring is great but who&#8217;s monitoring the monitor. It&#8217;s a classic problem. Instead of trying to make sure every monitoring solution you use is perfect, use multiple providers from multiple points of view (<a href="https://http/newrelic.com" target="_blank" rel="nofollow">application performance</a>, <a href="http://circonus.com/" target="_blank" rel="nofollow">system monitoring</a>, <a href="http://keynote.com/" target="_blank" rel="nofollow">synthetic polling</a>).

These perspectives all overlap to some degree backing each other up. If multiple providers start alerting, you know there is a real actionable problem and from how they alert, you can sometimes home in on the root cause much more quickly.

If your APM solution starts crying about CPU utilization but your system monitoring solution is silent, you know that you may have a problem that needs to be verified. Is the APM system misreading the situation or has your system monitoring agent failed to warn you of a serious issue?

### Mix and match private cloud

Regardless of all the above steps you can take to mitigate the risks of working in environments not completely in your control, really important business should remain in-house. You can keep the paradigm of software defined infrastructure by building a private cloud.

<a href="https://www.joyent.com/" target="_blank" rel="nofollow">Joyent</a> license their <a href="https://www.joyent.com/private-cloud" target="_blank" rel="nofollow">cloud platform</a> out to companies for building private clouds with enterprise support. This makes a mixing and matching between public and private very easy. In addition, they have <a href="https://github.com/joyent/sdc/" target="_blank" rel="nofollow">open sourced</a> the entire <a href="https://docs.joyent.com/sdc7/overview-of-smartdatacenter-7" target="_blank" rel="nofollow">cloud platform</a> so if you want to install without support, you are free to do so.

## Summary

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/p/7/005/0aa/170/0faa40a.jpg" alt="" width="322" height="222" data-loading-tracked="true" /> 

When a herd of elephants is stampeding, there is no hope of stopping them in their tracks. The best you can hope for is to point them in the right direction. Similarly, in the cloud, we will never get back the depth of visibility and control that we have with private deployments. What&#8217;s important is to learn how to steer the herd so we are prepared for the occasional stampede while still delivering high quality systems.