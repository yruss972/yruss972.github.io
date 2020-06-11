---
id: 378
title: 'Docker 2015 Wrap Up &#8211; The good, the bad and the ugly.'
date: 2016-01-13T08:43:30-07:00
author: Yonah Russ
layout: revision
guid: http://www.yonahruss.com/tips/376-revision-v1.html
permalink: /tips/376-revision-v1.html
---
<img class="aligncenter size-full wp-image-377" src="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/docker2015.jpg" alt="docker2015" width="698" height="393" srcset="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/docker2015.jpg 698w, http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/docker2015-300x169.jpg 300w" sizes="(max-width: 698px) 100vw, 698px" />

## Year in review

There is no question in my mind that Docker was, by far, the most disruptive technology of 2015. What barely registered on the radar for many in 2014, became something shiny in Q1, advanced to thinking material in Q2, reached &#8220;you have to be crazy to run that in production&#8221; by Q3, and is now on everyone&#8217;s three year plan.

## Getting some perspective

Sun released Solaris 10 and Zones around 2005. The performance advantages of OS level virtualization over hardware level virtualization were and are obvious but the real innovation came the combination of Zones with complementary technologies like ZFS. On the simplest level, quota support, delegated datasets, LOFI mounts, and snapshotting capabilities added layers upon layers of  functionality to containers.

By 2008, the only systems I had in dev, staging, or production that didn&#8217;t run on top of zones were Oracle RAC nodes. Every piece of code and every application server was deployed by sending an incremental ZFS snapshot of the container in staging to the servers in production. Effectively every server in production was running block level clones of the containers in staging.

It shouldn&#8217;t surprise anyone in the tech industry that we&#8217;ve come full circle as technology loves to do. Docker brings almost this exact pattern of shipping containers based on incremental file system changes to Linux but with some compelling twists and, yes, some troubling flaws.

## &#8230;the bad, and the ugly

The ugly truth is that the Linux container technology underlying Docker is incapable of providing true containerization. Unlike <a href="http://us-east.manta.joyent.com/jmc/public/opensolaris/ARChive/PSARC/2002/174/zones-design.spec.opensolaris.pdf" target="_blank">Solaris zones</a> which were designed from the beginning to provide complete isolation between containers, Linux containers are really just a combination of loosely coupled kernel features doing things they can, but weren&#8217;t necessarily meant to do, hob-cobbled together with a bunch of proverbial duct tape. The situation is surely improving with time, but if Linux containers continue to be developed as a disconnected combination of resources and their controls, I wouldn&#8217;t expect a mature solution for at least 10 years. That aside, if we put on our hear no evil/see no evil caps, Docker can get the job done in many cases.

## The good

### Container as a process

One of the most brilliant conceptual changes Docker has in contrast to what Sun had made possible with Zones and ZFS is the idea of a container as a process. I don&#8217;t know if this was originally intended by design or a happy consequence of building containerization on top of cgroups but it&#8217;s ground breaking.