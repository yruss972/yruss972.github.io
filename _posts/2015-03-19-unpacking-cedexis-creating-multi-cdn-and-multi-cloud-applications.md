---
id: 357
title: Unpacking Cedexis; Creating Multi-CDN and Multi-Cloud applications
date: 2015-03-19T01:37:19-07:00
author: Yonah Russ
layout: post
guid: http://www.yonahruss.com/?p=357
permalink: /architecture/unpacking-cedexis-creating-multi-cdn-and-multi-cloud-applications.html
dsq_thread_id:
  - "4487669442"
categories:
  - Architecture
  - Availability
  - Cloud
  - Performance
tags:
  - Anycast
  - Cloud computing
  - Cloud infrastructure
  - Content delivery network
  - DNS
  - multi-provider solutions
  - synthetic polling tool
  - Technology_Internet
---
<img class="aligncenter size-full wp-image-358" src="/assets/images/2016/01/package.jpg" alt="package" width="698" height="400" srcset="/assets/images/2016/01/package.jpg 698w, /assets/images/2016/01/package-300x172.jpg 300w" sizes="(max-width: 698px) 100vw, 698px" />

Technology is incredibly complex and, at the same time, incredibly unreliable. As a result, we build backup measures into the DNA of everything around us. Our laptops switch to battery when the power goes out. Our cellphones switch to cellular data when they lose connection to WiFi.

At the heart of this resilience, technology is constantly choosing between the available providers of a resource with the idea that if one provider becomes unavailable, another provider will take its place to give you what you want.

When the consumers are tightly coupled with the providers, for example, a laptop consuming power at Layer 1 or a server consuming LAN connectivity at Layer 2, the choices are limited and the objective, when choosing a provider, is primarily one of availability.

## Multiple providers for more than availability.

As multi-provider solutions make their way up the stack, however, additional data and time to make decisions enable choosing a provider based on other objectives like performance. Routing protocols, such as BGP, operate at Layer 3. They use path selection logic, not only to work around broken WAN connections but also to prefer paths with higher stability and lower latency.

As pervasive and successful as the multi-provider pattern is, many services fail to adopt a full stack multi-provider strategy. <a href="http://www.cedexis.com/" target="_blank">Cedexis</a> is an amazing service which has come to change that by making it trivial to bring the power of intelligent, real-time, provider selection your application.

I first implemented <a href="http://www.cedexis.com/openmix/multi-cdn.html" target="_blank">Multi-CDN</a> using Cedexis about 2 years ago. It was a no-brainer to go from Multi-CDN to Multi-Cloud. The additional performance, availability, and flexibility for the business became more and more obvious over time. Having a good multi-provider solution is key in cloud-based architectures and so I set out to write up a quick how-to on setting up a Multi-Cloud solution with Cedexis; but first you need to understand a bit about how Cedexis works.

## Cedexis Unboxed

Cedexis has a number of components:

  1. Radar
  2. OpenMix
  3. Sonar
  4. Fusion

### OpenMix

OpenMix is the brain of Cedexis. It looks like a DNS server to your users but, in fact, it is a multi-provider logic controller. In order to setup multi-provider solutions for our sites, we build OpenMix applications. Cedexis comes with the most common applications pre-built but the possibilities are pretty endless if you want something custom. As long as you can get the data you want into OpenMix, you can make your decisions based on that data in real time.

### Radar

Radar is where Cedexis really turned the industry on their heads. Radar uses a javascript tag to crowdsource billions of Real User Monitoring (RUM) metrics in real time. Each time a user visits a page with the Radar tag, they take a small number of random performance measurements and send the data back to Cedexis for processing.

The measurements are non-intrusive. They only happen several seconds after your page has loaded and you can control various aspects of what and how much gets tested by configuring the JS tag in the portal.

It’s important to note that Radar has two types of measurements:

  1. Community
  2. Private.

**Community Radar**

Community measurements are made against shared endpoints within each service provider. All Cedexis users that implement the Radar tag and allow community measurements get free access to the community Radar statistics. The community includes <a href="http://www.cedexis.com/reports/" target="_blank">statistics for the major Cloud compute, Cloud storage, and CDNs</a> making Radar the first place I go to research developments and trends in the Cloud/CDN markets.

Community Radar is the fastest and easiest way to use Cedexis out of the box and the community measurements also have the most volume so they are very accurate all the way up to the “door” of your service provider. They do have some disadvantages though.

The community data doesn’t account for performance changes specific to each of the provider’s tenants. For example, community Radar for Amazon S3 will gather RUM data for accessing a test bucket in the specified region. This data assumes that within the S3 region all the buckets perform equally.

Additionally, there are providers which may opt out of community measurements so you might not have community data for some providers at all. In that case, I suggest you try to connect between your account managers and get them included. Sometimes it is just a question of demand.

**Private Radar**

Cedexis has the ability to configure custom Radar measurements as well. These measurements will only be taken by your users, the ones using your JS tag.

Private Radar lets you monitor dedicated and other platforms which aren’t included in the community metrics. If you have enough traffic, private Radar measurements have the added bonus of being specific to your user base and of measuring your specific application so the data can be even more accurate than the community data.

The major disadvantage of private Radar is that low volume metrics may not produce the best decisions. With that in mind, you will want to supplement your data with other data sources. I’ll show you how to set that up.

### Situational Awareness

More than just a research tool, Radar makes all of these live metrics available for decision-making inside OpenMix. That means we can make much more intelligent choices than we could with less precise technologies like Geo-targeting and Anycast.

Most people using Geo-targeting assume that being geographically close to a network destination is also closer from the networking point of view. In reality, network latency depends on many factors like available bandwidth, number of hops, etc. Anycast can pick a destination with lower latency, but it’s stuck way down in Layer 3 of the stack with no idea about application performance or availability.

With Radar, you get real-time performance comparisons of the providers you use, from your user’s perspectives. You know that people on ISP Alice are having better performance from the East coast DC while people on ISP Bob are having better performance from the Midwest DC even if both these ISPs are serving the same geography.

### Sonar

Whether you are using community or low volume private Radar measurements, you ideally want to try and get more application specific data into OpenMix. One way to do this is with Sonar.

Sonar is a synthetic polling tool which will poll any URL you give it from multiple locations and store the results as availability data for your platforms. For the simplest implementation, you need only an address that responds with an OK if everything is working properly.

If you want to get more bang for your buck, you can make that URL an intelligent endpoint so that if your platform is nearing capacity, you can pretend to be unavailable for a short time to throttle traffic away before your location really has issues.

You can also use the Sonar endpoints as a convenient way to automate diverting traffic for maintenance windows- No splash pages required.

### Fusion

Fusion is really another amazing piece of work from Cedexis. As you might guess from its name, Fusion is where Cedexis glues services together and it comes in two flavors:

  1. Global Purge
  2. Data Feeds

**Global Purge**

By nature, one of the most apropos uses of Cedexis is to marry multiple CDN providers for better performance and stability. Every CDN has countries where they are better and countries where they are worse. In addition, maintenance windows in a CDN provider can be devastating for performance even though they usually won’t cause downtime.

The downside of a Multi-CDN approach is the overhead involved in managing each of the CDNs and most often that means purging content from the cache. Fusion allows you to connect to multiple supported CDN providers (a lot of them) and purge content from all of them from one interface inside Cedexis.

While this is a great feature, I have to add that you shouldn’t be using it. Purging content from a CDN is very Y2K and you should be using versioned resources with far futures expiry headers to get the best performance out of your sites and so you never have to purge content from a CDN ever again.

**Data Feeds**

This is the really great part. Fusion lets you import data from basically anywhere to use in your OpenMix decision making process. Built in, you will find connections to various CDN and monitoring services, but you can also work with Cedexis to setup custom Fusion integrations so the sky&#8217;s the limit.

With Radar and Sonar, we have very good data on performance and availability (time and quality) both from the real user perspective and a supplemental synthetic perspective. To really optimize our traffic we need to account for all three corners of the Time, Cost, Quality triangle.

With Fusion, we can introduce cost as a factor in our decisions. Consider a company using multiple CDN providers, each with a minimum monthly commitment of traffic. If we would direct traffic based on performance alone, we might not meet the monthly commitment on one provider but be required to pay for traffic we didn’t actually send. Fusion provides usage statistics for each CDN and allows OpenMix to divert traffic so that we optimize our spending.

## Looking Forward

With all the logic we can build into our infrastructure using Cedexis, it could almost be a fire and forget solution. That would, however, be a huge waste. The Internet is always evolving. Providers come and go. Bandwidth changes hands.

Cedexis reports provide operational intelligence on alternative providers without any of the hassle involved in a POC. Just plot the performance of the provider you’re interested in against the performance of your current providers and make an informed decision to further improve your services. When something better come along, you’ll know it.

## The Nitty Gritty

Keep an eye out for the next article where I’ll do a step by step walk-through on <a href="http://www.yonahruss.com/tips/how-to-setup-a-multi-platform-website-with-cedexis.html" target="_blank" rel="nofollow">setting up a Multi-Cloud solution using Cedexis</a>. I’ll cover almost everything mentioned here, including Private and Community Radar, Sonar, Standard and Custom OpenMix Applications, and Cedexis Reports.