---
id: 352
title: Virtual Block Storage Crashed Your Cloud Again :(
date: 2015-02-10T09:49:23-07:00
author: Yonah Russ
layout: post
guid: http://www.yonahruss.com/?p=352
permalink: /storage/virtual-block-storage-crashed-your-cloud-again.html
dsq_thread_id:
  - "4487739902"
categories:
  - Availability
  - Cloud
  - Storage
tags:
  - Amazon Elastic Block Store
  - Amazon Web Services
  - block devices
  - Cloud computing
  - Cloud infrastructure
  - Cloud storage
  - EBS
  - HP Cloud
  - Netflix
  - on-metal performance
  - open source private cloud solutions
  - OpenStack
  - persistent block storage solution
  - Rackspace
  - software bug
  - Technology_Internet
  - Web hosting
  - Web services
---
<img class="aligncenter size-full wp-image-353" src="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/darthvader.jpg" alt="darthvader" width="698" height="400" srcset="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/darthvader.jpg 698w, http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/darthvader-300x172.jpg 300w" sizes="(max-width: 698px) 100vw, 698px" />

You know it&#8217;s bad when you start writing an incident report with the words &#8220;The first 12 hours.&#8221; You know you need a stiff drink, possibly a career change, when you follow that up with phrases like &#8220;this was going to be a lengthy outage&#8230;&#8221;, &#8220;the next 48 hours&#8230;&#8221;, and &#8220;as much as 3 days&#8221;.

That&#8217;s what happened to huge companies like <a href="http://techblog.netflix.com/2011/04/lessons-netflix-learned-from-aws-outage.html" target="_blank" rel="nofollow">NetFlix</a>, <a href="https://status.heroku.com/incidents/151" target="_blank" rel="nofollow">Heroku</a>, <a href="https://www.reddit.com/comments/gva4t/on_reddits_outage/" target="_blank" rel="nofollow">Reddit</a>,<a href="http://blog.hootsuite.com/notes-on-todays-outage/" target="_blank" rel="nofollow">Hootsuite</a>, Foursquare, Quora, and Imgur the week of April 21, 2011. <a href="https://aws.amazon.com/message/65648/" target="_blank" rel="nofollow">Amazon AWS went down for over 80 hours</a>, leaving them and others up a creek without a paddle. The root cause of this cloud-tastrify echoed loud and clear.

<a href="https://status.heroku.com/incidents/151" target="_blank" rel="nofollow">Heroku</a> said:

> The biggest problem was our use of EBS drives, AWS&#8217;s persistent block storage solution&#8230; **Block storage is not a cloud-friendly technology.** EC2, S3, and other AWS services have grown much more stable, reliable, and performant over the four years we&#8217;ve been using them. EBS, unfortunately, has not improved much, and in fact has possibly gotten worse. Amazon employs some of the best infrastructure engineers in the world: if they can&#8217;t make it work, then probably no one can.

<a href="https://www.reddit.com/comments/gva4t/on_reddits_outage/" target="_blank" rel="nofollow">Reddit</a> said:

> Amazon had a failure of their EBS system, which is a data storage product they offer, at around 1:15am PDT. This may sound familiar, because it was the same type of failure that took us down a month ago. This time however the failure was more widespread and affected a much larger portion of our servers

While most companies made heartfelt resolutions to get off of EBS, <a href="http://techblog.netflix.com/2011/04/lessons-netflix-learned-from-aws-outage.html" target="_blank" rel="nofollow">NetFlix</a> was clear to point out that they never trusted EBS to begin with:

> When we re-designed for the cloud this Amazon failure was exactly the sort of issue that we wanted to be resilient to. Our architecture avoids using EBS as our main data storage service&#8230;

## Fool me once&#8230;

As Reddit mentioned in their postmortem, AWS had <a href="http://www.redditblog.com/2011/03/why-reddit-was-down-for-6-of-last-24.html" target="_blank" rel="nofollow">similar EBS problems</a> twice before on a smaller scale in March. After an additional 80+ hours of downtime, you would expect companies to learn their lesson, but the facts are that these same outages continue to plague clouds using various types of virtual block storage.

In July 2012, <a href="https://aws.amazon.com/message/67457/" target="_blank" rel="nofollow">AWS experienced a power failure</a> which resulted in a huge number of possibly inconsistent EBS volumes and an overloaded control plane. Some customers experienced almost 24 hours of downtime.

<a href="https://status.heroku.com/incidents/386" target="_blank" rel="nofollow">Heroku</a>, under the gun again, said:

> Approximately 30% of our EC2 instances, which were responsible for running applications, databases and supporting infrastructure (including some components specific to the Bamboo stack), went offline&#8230;  
> A large number of EBS volumes, which stored data for Heroku Postgres services, went offline and their data was potentially corrupted&#8230;  
> 20% of production databases experienced up to 7 hours of downtime. A further 8% experienced an additional 10 hours of downtime (up to 17 hours total). Some Beta and shared databases were offline for a further 6 hours (up to 23 hours total).

<a href="http://blog.appharbor.com/2012/07/05/june-29th-aws-service-event-post-mortem" target="_blank" rel="nofollow">AppHarbor</a> had similar problems:

> EC2 instances and EBS volumes were unavailable and some EBS volumes became corrupted&#8230;  
> Unfortunately, many instances were restored without associated EBS volumes required for correct operation. When volumes did become available they would often take a long time to properly attach to EC2 instances or refuse to attach altogether. Other EBS volumes became available in a corrupted state and had to be checked for errors before they could be used.  
> &#8230;a software bug prevented this fail-over from happening for a small subset of multi-az RDS instances. Some AppHarbor MySQL databases were located on an RDS instance affected by this problem.

The saga continues for AWS who <a href="https://aws.amazon.com/message/680342/" target="_blank" rel="nofollow">continued to have problems with EBS</a> later in 2012. They detail ad nauseam, how a small DNS misconfiguration triggered a memory leak which caused a silent cascading failure of all the EBS servers. As usual, the EBS failures impacted API access and RDS services. Yet again Multi-AZ RDS instances didn&#8217;t failover automatically.

## Who&#8217;s using Virtual Block Storage?

Amazon EBS is just one very common example of Virtual Block Storage and by no means, the only one to fail miserably.

Azure stores the block devices for all their compute nodes as Blobs in their premium or standard storage services. Back in November, a bad update to the storage service sent some of their storage endpoints into infinite loops, denying access to many of these virtual hard disks. The bad software was deployed globally and caused more than <a href="http://azure.microsoft.com/blog/2014/11/19/update-on-azure-storage-service-interruption/" target="_blank" rel="nofollow">10 hours of downtime across 12 data centers</a>. According to the post, some customers were still being affected as much as three days later.

HP Cloud provides virtual block storage based on <a href="https://wiki.openstack.org/wiki/Cinder" target="_blank" rel="nofollow">OpenStack Cinder</a>. See related incident reports <a href="https://community.hpcloud.com/status/incident/2843" target="_blank" rel="nofollow">here</a>, <a href="https://community.hpcloud.com/status/incident/2809" target="_blank" rel="nofollow">here</a>, <a href="https://community.hpcloud.com/status/incident/2808" target="_blank" rel="nofollow">here</a>, <a href="https://community.hpcloud.com/status/maintenance/2802" target="_blank" rel="nofollow">here</a>, <a href="https://community.hpcloud.com/status/incident/2791" target="_blank" rel="nofollow">here</a>. I could keep going back in time, but I think you get the point.

Also based on Cinder, Rackspace offers their <a href="http://www.rackspace.com/cloud/block-storage" target="_blank" rel="nofollow">Cloud Block Storage</a> product. Their solution has some proprietary component they call Lunr, as detailed in this <a href="https://news.ycombinator.com/item?id=4687874" target="_blank" rel="nofollow">Hacker News thread</a> so you can hope that Lunr is more reliable than other implementations. Still, Rackspace had <a href="https://status.rackspace.com/index/viewincidents?group=11&start=1401595200" target="_blank" rel="nofollow">major capacity issues</a> spanning over two weeks back in May of last year and I shudder to think what would have happened if anything went wrong while capacity was low.

Storage issues are so common and take so long to recover from in OpenStack deployments, that companies are deploying <a href="https://ops.faithlife.com/?p=6" target="_blank" rel="nofollow">alternate cloud platforms as a workaround</a> while their OpenStack clouds are down.

## What clouds won&#8217;t ruin your SLA?

<a href="http://www.rackspace.com/" target="_blank" rel="nofollow">Rackspace</a> doesn&#8217;t force you to use their Cloud Block Storage, at least not yet, so unless they are drinking their own kool-aid in ways they shouldn&#8217;t be, you are hopefully safe there.

<a href="https://www.digitalocean.com/" target="_blank" rel="nofollow">Digital Ocean</a> also relies on local block storage by design. They are apparently <a href="http://digitalocean.uservoice.com/forums/136585-digital-ocean/suggestions/3127077-extra-diskspace-" target="_blank" rel="nofollow">considering other options</a> but want to <a href="http://digitalocean.uservoice.com/forums/136585-digital-ocean/suggestions/3127077-extra-diskspace-?page=13&per_page=20" target="_blank" rel="nofollow">avoid an EBS-like solution</a> for the reasons I&#8217;ve mentioned. While their local storage isn&#8217;t putting you at risk of a cascading failure, they have been <a href="https://news.ycombinator.com/item?id=6983097" target="_blank" rel="nofollow">reported to leak your data to other customers</a> if you don&#8217;t destroy your machines carefully. They also have other fairly <a href="https://status.digitalocean.com/" target="_blank" rel="nofollow">frequent</a> <a href="https://cloudharmony.com/provider/digitalocean" target="_blank" rel="nofollow">issues</a>which take them out of the running for me.

## The winning horse

As usual, <a href="https://www.joyent.com/" target="_blank" rel="nofollow">Joyent</a> shines through on this. For <a href="https://www.joyent.com/blog/network-storage-in-the-cloud-delicious-but-deadly/" target="_blank" rel="nofollow">many reasons</a>, the <a href="http://www.yonahruss.com/architecture/cloud/smartdatacenter-the-open-cloud-platform-that-actually-already-works.html" target="_blank" rel="nofollow">SmartDataCenter platform</a>, behind both their public cloud and open source private cloud solutions, supports only local block storage. For centralized storage, you can use NFS or CIFS if you really need to but you will not find virtual block storage or even SAN support.

Joyent gets some flack for this <a href="https://www.joyent.com/blog/smartdatacenter-and-the-merits-of-being-opinionated" target="_blank" rel="nofollow">opinionated architecture</a>, occasionally even from me, but they don&#8217;t corrupt my data or crash my servers because some virtual hard disk has gone away or some software upgrade has been foolishly deployed.

With their recently released <a href="http://www.yonahruss.com/unix/linux-and-solaris-are-converging-but-not-the-way-you-imagined.html" target="_blank" rel="nofollow">Docker and Linux Binary support</a>, Joyent is really leading the pack with on-metal performance and availability. I definitely recommend hitching your wagon to their horse.

## The Nooooooooooooooo! button

If it&#8217;s too late and you&#8217;re only finding this article post cloud-tastrify, I refer you to the ever amusing <a href="http://nooooooooooooooo.com/" target="_blank" rel="nofollow">Nooooooooooooooo! button</a> for some comic relief.