---
id: 340
title: SmartDataCenter, the Open Cloud Platform that Actually Already Works
date: 2015-01-18T09:22:14-07:00
author: Yonah Russ
layout: post
guid: http://www.yonahruss.com/?p=340
permalink: /architecture/cloud/smartdatacenter-the-open-cloud-platform-that-actually-already-works.html
categories:
  - Cloud
tags:
  - Cloud computing
  - Cloud infrastructure
  - container technology
  - internal infrastructure
  - linux
  - on-metal performance
  - OpenSolaris
  - OpenStack
  - SmartOS
  - SMF
  - solaris
---
<img class="aligncenter size-full wp-image-341" src="/assets/images/2016/01/sdc.jpg" alt="sdc" width="698" height="400" srcset="/assets/images/2016/01/sdc.jpg 698w, /assets/images/2016/01/sdc-300x172.jpg 300w" sizes="(max-width: 698px) 100vw, 698px" />

For years enterprises have tried to make OpenStack work and failed miserably. Considering how many heads have broken against OpenStack, maybe they should have called it OpenBrick.

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/p/8/005/0af/2d3/1a6a888.jpg" alt="" width="595" height="223" data-loading-tracked="true" /> 

Before I dive into the details, I&#8217;ll cut to the chase. You don&#8217;t have to break your heads on cloud anymore. <a href="https://www.joyent.com/private-cloud" target="_blank" rel="nofollow">Joyent</a> have <a href="https://www.joyent.com/blog/sdc-and-manta-are-now-open-source" target="_blank" rel="nofollow">open sourced</a> (as in get it on <a href="https://github.com/joyent/sdc/" target="_blank" rel="nofollow">Github</a>) their cloud management platform.

It&#8217;s free if you want (<a href="https://github.com/joyent/sdc/#cloud-on-a-laptop-coal" target="_blank" rel="nofollow">install it on your laptop</a>, <a href="https://github.com/joyent/sdc/#installing-sdc-on-a-physical-server" target="_blank" rel="nofollow">install it on a server</a>). It&#8217;s supported if you want. Best of all, it actually works outside of a lab or CI test suite. It&#8217;s what Joyent runs in production for all their public cloud customers (I admit to being one of the satisfied ones). It&#8217;s also something they have been licensing out to other cloud providers for years.

Now for the deep dive.

## What&#8217;s wrong with OpenStack?

First off, it isn&#8217;t a cloud in a box, which is what most people think it is. In 2013,<a href="http://blogs.gartner.com/alessandro-perilli/why-vendors-cant-sell-openstack-to-enterprises/" target="_blank" rel="nofollow">Gartner</a> called out OpenStack for consciously misrepresenting what OpenStack actually provides:

> no one in three years stood up to clarify what OpenStack can and cannot do for an enterprise.

In case you&#8217;re wondering, the analyst also quoted Ebay&#8217;s chief engineer on the true nature of OpenStack:

> &#8230; an instance of an OpenStack installation does not make a cloud. As an operator you will be dealing with many additional activities not all of which users see. These include infra onboarding, bootstrapping, remediation, config management, patching, packaging, upgrades, high availability, monitoring, metrics, user support, capacity forecasting and management, billing or chargeback, reclamation, security, firewalls, DNS, integration with other internal infrastructure and tools, and on and on and on. These activities are bound to consume a significant amount of time and effort. OpenStack gives some very key ingredients to build a cloud, but it is not cloud in a box.

The analyst made it clear that:

> vendors get this difference, trust me.

Other <a href="https://stochasticresonance.wordpress.com/2013/11/04/openstack-a-plea/" target="_blank" rel="nofollow">insiders</a> put the situation into similar terms:

> OpenStack has some success stories, but dead projects tell no tales. I have seen no less than 100 Million USD spent on bad OpenStack implementations that will return little or have net negative value.<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/p/7/005/0af/2d1/29b0aa0.jpg" alt="" width="458" height="343" data-loading-tracked="true" />Some of that has to be put on the ignorance and arrogance of some of the organizations spending that money, but OpenStack’s core competency, above all else, has been marketing and if not culpable, OpenStack has at least been complicit.

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/p/7/005/0af/2d2/3374795.jpg" alt="" width="302" height="227" data-loading-tracked="true" /> The motive behind the deception is clear. OpenStack is like giving someone a free Ferrari, pink slip and all but keeping the keys. You get pieces of a cloud but no way to run it. Once you have put all your effort into installing OpenStack and you realize what&#8217;s missing, you are welcome to turn to any one of the vendors backing OpenStack for one of their packaged cloud platforms.

OpenStack is a foot in the door. It&#8217;s a classic bait and switch but even after years, no one is admitting it. Instead, blue chip companies fight to steer OpenStack into the direction that suits them and their corporate offerings.

## What&#8217;s great about SmartDataCenter?

###<img class="center" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/p/5/005/0af/2d5/09112c2.jpg" alt="" width="588" height="137" data-loading-tracked="true" /> 

### It works.

The keys are in the ignition. You should probably stop reading this article and install it already. You are likely to get a promotion for listening to me 😉

### Great Technology

SmartDataCenter was built on really great technologies like SmartOS (fork of Solaris), Zones, ZFS, and DTrace. Most of these technologies are slowly being ported to Linux but they are already 10 years mature in SDC.

  * Being based on a fork of Solaris brings you baked in enterprise ready features like <a href="http://www.c0t0d0s0.org/archives/4087-Less-known-Solaris-features-IPsec.html" target="_blank" rel="nofollow">IPSEC</a>, IPF, <a href="http://www.c0t0d0s0.org/archives/4077-Less-known-Solaris-features-RBAC-and-Privileges.html" target="_blank" rel="nofollow">RBAC</a>, <a href="http://www.c0t0d0s0.org/archives/4147-Solaris-Features-Service-Management-Facility-Part-4-Developing-for-SMF.html" target="_blank" rel="nofollow">SMF</a>, <a href="http://www.c0t0d0s0.org/archives/4211-Less-known-Solaris-Features-Resource-Management.html" target="_blank" rel="nofollow">Resource management and capping</a>, <a href="http://www.c0t0d0s0.org/archives/4068-Less-known-Solaris-features-Auditing.html" target="_blank" rel="nofollow">System auditing</a>, <a href="http://www.c0t0d0s0.org/archives/4069-Less-known-Solaris-features-BART.html" target="_blank" rel="nofollow">Filesystem monitoring</a>, etc.
  * Zones are the big daddy of container technology guaranteeing you the best on-metal performance for your cloud instances. If you are running a native SmartOS guest, you get the added benefit of CPU bursting and live machine re-size (no reboot, or machine pause necessary).
  * ZFS is the most reliable, high performance, file system in the world and is constantly improving.
  * DTrace is the secret to low level visibility with zero to no overhead. In cloud deployments where visibility is usually close to zero, this is an amazing feature. It&#8217;s even more amazing as the cloud operator.

### Focus

SDC was built for one thing by one company, to replace the data centers of the past. It says so in the name. With one purpose, SDC has been built to be very<a href="https://github.com/joyent/sdc/#design-principles" target="_blank" rel="nofollow">opinionated</a> about what it does and how it does it. This gives SDC a tremendous amount of focus, something sorely lacking from would-be competition like OpenStack.

## Lastly, it works.