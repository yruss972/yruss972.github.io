---
id: 376
title: 'Docker 2015 Recap &#8211; The good, the bad and the ugly.'
date: 2016-01-19T02:48:35-07:00
author: Yonah Russ
layout: post
guid: http://www.yonahruss.com/?p=376
permalink: /architecture/virtualization/docker-2015-recap-the-good-the-bad-and-the-ugly.html
categories:
  - Virtualization
tags:
  - bare metal
  - configuration management tools
  - container technology
  - disruptive technology
  - Docker
  - Joyent
  - linux
  - Linux container technology
  - mature container solutions
  - solaris
  - Technology_Internet
  - Triton
---
<img class="aligncenter size-full wp-image-379" src="/assets/images/2016/01/docker2015me.jpg" alt="docker 2015" width="698" height="393" srcset="/assets/images/2016/01/docker2015me.jpg 698w, /assets/images/2016/01/docker2015me-300x169.jpg 300w" sizes="(max-width: 698px) 100vw, 698px" />

## Year in review

There is no question in my mind that Docker was, by far, the most disruptive technology of 2015. What barely registered on the radar for many in 2014, became something shiny in Q1, advanced to thinking material in Q2, reached &#8220;you have to be crazy to run that in production&#8221; by Q3, and is now on everyone&#8217;s three year plan.

To make a long story short:

  1. Containers were a great idea 12 years ago and Docker has brought containers to Linux and managed to improve on the concept.
  2. That said, the actual implementation of containers in Linux is a disaster. If you can&#8217;t use Triton or another container technology, be prepared to step on a lot of mines.
  3. In proper hands and under proper circumstances (dev, qa, limited production use), Docker is already useful so use it.
  4. Though it&#8217;s production capable, it&#8217;s not production ready. Parental supervision required.

## Getting some perspective

Sun released Solaris 10 and Zones around 2005. The performance advantages of OS level virtualization over hardware level virtualization were and are obvious but the real innovation came the combination of Zones with complementary technologies like ZFS. On the simplest level, quota support, delegated datasets, LOFI mounts, and snapshotting capabilities added layers upon layers of  functionality to containers.

By 2008, the only systems I had in dev, staging, or production that didn&#8217;t run on top of zones were Oracle RAC nodes. Every piece of code and every application server was deployed by sending an incremental ZFS snapshot of the container in staging to the servers in production. Effectively every server in production was running block level clones of the containers in staging.

It shouldn&#8217;t surprise anyone in the tech industry that we&#8217;ve come full circle as technology loves to do. Docker brings almost this exact pattern of shipping containers based on incremental file system changes to Linux but with some compelling twists and, yes, some troubling flaws.

## 

## The good

### Container as an artifact

Developers rarely keep a clean house and by that I mean a clean working environment. Working on multitudes of projects they&#8217;ll often have multiple versions of a runtime installed, not to mention any number of libraries. When software is written in a dirty environment, dependencies can be forgotten or met unintentionally and it leads to &#8220;worked on my laptop syndrome.&#8221; One of the major advantages of containers is the filesystem level isolation for the processes. When developers work properly with containers, their software, runtimes, and dependencies are all isolated from any other project. The same code and dependencies running in dev, go to staging, and then possibly even to production.

### Container as a process

One of the most brilliant conceptual changes Docker has, in contrast to what Sun had made possible with Zones and ZFS, is the idea of a container as a process. I don&#8217;t know if this was originally intended by design or a happy consequence of building containerization on top of cgroups but it&#8217;s ground breaking for several reasons:

  1. Running a single process per container has significantly less resource requirements than running a traditional fat container like a Solaris Zone.
  2. Running a single process per container has significantly less attack vectors than running a traditional fat container.
  3. The paradigm of a container accepting STDIN and returning STDOUT/STDERR like a normal process brings <a href="https://en.wikipedia.org/wiki/Unix_philosophy" target="_blank">the UNIX philosophy</a> to new heights, allowing, not just software, but even very complex processes to be packaged as containers and piped together.

### API

While the lean, process model container is one of the best innovations Docker brings to the table, it is not without it&#8217;s downsides. Docker containers are poorly suited for services which require multiple running processes, involve complex configurations, and/or work with many files/output channels. Mounting configuration, data, or log files from the Docker host can work around some of these issues but that strays from the general goal of having a self contained, self reliant image which can be run anywhere.

The alternative is to pass some unwieldy combination of environment variables to the container at runtime. On the command line, this is infuriating after a while but when managed by API, it becomes much more tractable. The API also makes it fairly straightforward to replace cloud resources with Docker resources so configuration management tools like Chef, Puppet, Ansible, and Vagrant have already been able to provide reasonably mature support despite the still growing adoption levels.

That said, the most innovative use of the Docker API has been <a href="http://www.yonahruss.com/tips/triton-bare-metal-containers-ftw.html" target="_blank">Joyent&#8217;s Triton</a> offering. By creating a virtual Docker endpoint as a gateway to the Joyent public cloud, they have become the only service, that I&#8217;m aware of, which runs Docker containers on bare metal without maintaining dedicated Docker Host machines. Having built and run a production SAAS based on Docker, I can&#8217;t stress enough how much of a game changer that is in terms of optimal resource utilization, and consequently in terms of price.

### Layers

While ZFS technically enabled Zones to be built from incremental changes to a file system, there was really very little, if any integration there. I suppose you could name snapshots but I don&#8217;t think there was any way to get a history of the layers involved in creating the container like there is with Docker.

With ZFS+deduplication you could also achieve something along the lines of Docker&#8217;s caching and reuse of layers. To be honest, in most cases I preferred not to enable deduplication and to have another copy of my data in production. In development, however, if you want to run 100 containers based on Ubuntu, you are probably more than happy to reuse the Ubuntu layer.

Another compelling pattern for layer reuse is the data volume container. Assuming a service relying on a database, you could store the initial database in an image and then run any number of data volume containers based on that image. Each container will contain a private version of the data but only store the changes from the original image which is excellent for development, automated testing, etc.

With the properly constructed Dockerfile, caching layers can also significantly reduce the amount of time involved in downloading images based on layers which have already been downloaded, updating containers with new changes, etc. That comes with several caveats that layer caching is not infallible and you may easily miss updating critical pieces of your software if you do it wrong, for examples <a href="https://github.com/docker/docker/issues/13331" target="_blank">see here</a> and <a href="https://github.com/docker/docker/issues/13331" target="_blank">here</a>.

## &#8230;the bad, and the ugly

The ugly truth is that the Linux container technology underlying Docker is incapable of providing true containerization. Unlike <a href="http://us-east.manta.joyent.com/jmc/public/opensolaris/ARChive/PSARC/2002/174/zones-design.spec.opensolaris.pdf" target="_blank">Solaris zones</a> which were designed from the beginning to provide complete isolation between containers, Linux containers are really just a combination of loosely coupled kernel features doing things they can, but weren&#8217;t necessarily meant to do, hob-cobbled together with a bunch of proverbial duct tape.

Once, after a service upgrade, we experienced transient failures that crippled our system. It took about 6 hours to find a rogue instance of the previous application that had remained running although the &#8220;container&#8221; it was running was deleted. Since there is no real container entity in Linux, you will often end up with orphaned file system mounts, and less often, broken iptables rules and rogue processes.

Another major issue is the lack of &#8220;C4I&#8221; solutions/patterns in the Docker space. In a fat host/VM environment, services rely on any number of base services provided by the host including DNS resolution, email, logging, time synchronization, firewall, etc. In the lean environments of process model containers, there is nothing to rely on. In many cases, organizations fall back to providing non-containerized solutions from the host. In other cases, deploying privileged agent containers alongside application containers is used. Many times a combination of both is necessary but none of these solutions is optimal.

Most importantly, Docker when used improperly can add numerous attack surfaces to any system. You must be extremely careful with access to the docker daemon whether via TCP or via socket. Helpers like docker-machine are so focused on providing the easiest out-of-box experience that it&#8217;s too easy to do something wrong. You must be vigilant regarding the trustworthiness and the maintenance of container contents. With DockerHub and Docker, the instant gratification of pulling and running a basic instance of any software can quickly erase any common sense regarding the trustworthiness of the software inside. Has the software been updated or am I opening myself to known exploits? Has the software been back-doored? <a href="https://medium.com/@ewindisch/evil-docker-images-25a941c7d5a7#.omuoqiwwi" target="_blank">Will the container be mining bitcoins for someone else using my resources?</a>

## In summary

Docker has both good and bad points but the situation is surely improving with time. If Linux containers continue to be developed as a disconnected combination of resources and their controls, I wouldn&#8217;t expect a mature Linux based solution for at least 10 years.

For the long run, a better and faster alternative would be to keep Docker&#8217;s conceptual innovations and API while replacing the underlying technology with adaptations of mature container solutions like Zones. Joyent is already playing a visionary role on that front. Their focus on cloud, however, means that there are no plans in sight to support a more standard zones based Docker Host. I wish someone in the space would pick up the gauntlet there.

In short, if you can run on Triton/Solaris derivatives with Zones, that would be my first pick. If you have to run on Linux, tread carefully, resist the temptation for instant gratification,  and Docker on Linux can already be useful in many cases.