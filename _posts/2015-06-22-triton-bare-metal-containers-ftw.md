---
id: 370
title: Triton Bare Metal Containers FTW!
date: 2015-06-22T04:24:34-07:00
author: Yonah Russ
layout: post
guid: http://www.yonahruss.com/?p=370
permalink: /tips/triton-bare-metal-containers-ftw.html
dsq_thread_id:
  - "4487151411"
categories:
  - Cloud
  - Performance
  - Tips
tags:
  - bare metal
  - Cgroups
  - Docker
  - FreeBSD
  - linux
  - LXC
  - OpenSolaris
  - Operating-system-level virtualization
  - solaris
  - Vagrant
  - VirtualBox
  - VMWare
  - Xen
  - zone technology
---
<img class="aligncenter size-full wp-image-371" src="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/triton.jpg" alt="triton" width="698" height="400" srcset="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/triton.jpg 698w, http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/triton-300x172.jpg 300w" sizes="(max-width: 698px) 100vw, 698px" />

If you haven&#8217;t heard of <a href="https://www.docker.com/" target="_blank" rel="nofollow">Docker</a>, I&#8217;m not sure what cave you&#8217;ve been living in but here&#8217;s the short story:

Hardware level virtualization, like you are used to (VMWare, Virtualbox, KVM, Xen, Amazon, Rackspace, Azure, etc.) is slow and awful. Virtualization at the OS level, where processes share isolated access to a single Operating System kernel, is much more efficient and therefore awesome. Another way of saying OS level virtualization is &#8220;containers&#8221; such as have existed in FreeBSD and Solaris for over a decade.

Docker is hacking together a whole slew of technologies (cgroups, union filesystems, iptables, etc.) to finally bring the concept of containers to the Linux masses. Along the way, they&#8217;ve also managed to evolve the concept a little adding the idea of running a container as very portable unit which runs as a process.

Instead of managing dependencies across multiple environments and platforms, an ideal Docker container encapsulates all the runtime dependencies for a service or process. Instead including a full root file system, the ideal Docker container could be as small as a single binary. Taking that hand in hand with the growing interest in developing reusable microservices, we have an amazing tech revolution on our hands.

That is not to say that everything in the Docker world is all roses. I did say that Docker is hacking together a slew of technologies so while marketing and demos portray:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAANpAAAAJGI4ZmJhMDA4LTE1YjgtNDg1My05MTE2LWFkNDU3ODYzODcwZQ.jpg" alt="" width="588" height="367" data-loading-tracked="true" /> You are more likely to start with something like this:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAANJAAAAJGE2OGViZWIzLTAwMTItNGFjZC1hODYxLTQzZjhlMjQyZmIxOA.jpg" alt="" width="588" height="392" data-loading-tracked="true" /> 

Either way, tons of containers per host is great until you realize you are lugging them around on a huge, slow, whale of a cargo ship.

  1. Currently, Docker&#8217;s level of isolation between containers is not that great so security and noisy neighbors are issues.
  2. Containers on the same host can&#8217;t easily run on the same ports so you may have to do some spaghetti networking.
  3. On top of that, if you are running Docker in Amazon, Google, Azure, etc. you are missing the whole point which was to escape the HW level virtualization.

## Joyent to the rescue!

Joyent is the only container based cloud provider that I&#8217;m aware of. They have been running the vast majority of my cloud instances (possibly yours as well) on OS level virtualization for years now (years before Docker was a twinkle in Shamu&#8217;s eye). As such, they are possibly the most experienced and qualified technical leaders on the subject.

They run a customized version of Illumos, an OpenSolaris derivative, with extremely efficient zone technology for their containers. In January (<a href="http://www.yonahruss.com/unix/linux-and-solaris-are-converging-but-not-the-way-you-imagined.html" target="_blank" rel="nofollow">Linux and Solaris are Converging but Not the Way you Think</a>), I wrote about the strides Joyent made allowing Linux binaries to run inside Illumos zones.

## Triton May Qualify as Witchcraft

The love child of that work, announced as GA last week, was Triton- a Docker API compliant (for the most part) service running zone based Docker containers on bare metal in the cloud. If running on bare metal weren&#8217;t enough of an advantage, Joyent completely abstracted away the notion of the Docker host (ie. the cargo ship). Instead, your Docker client speaks to an API endpoint which schedules your bare metal containers transparently across the entire cloud.

Addressing each of the points I mentioned above:

  1. Zones in Illumos/Joyent provide complete isolation as opposed to Linux based containers so no security or noisy neighbor problems.
  2. Every container gets it&#8217;s own public ip address with a full range of ports so no spaghetti networking
  3. No Docker host and no HW virtualization so every container is running full speed on bare metal

Going back to the boat analogy, if Docker containers on Linux in most clouds looks like this:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAI9AAAAJDAxMjVhZWI1LTQyZmEtNDNmOC05MmExLWYyNTcwMGE1MDlmZA.jpg" alt="" width="588" height="392" data-loading-tracked="true" /> 

Docker containers as zones on bare metal in Joyent look like this:

##<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAANiAAAAJDZkZjAyOTg2LWU5ZTAtNGE2OC05YzYxLTk4MDdjZDNiZWJiOQ.jpg" alt="" width="588" height="338" data-loading-tracked="true" /> Enough of the FUD

I&#8217;m not a big fan of marketing FUD so I&#8217;ve been kicking the tires on the beta version of Triton for a while. Now with the GA, here are the pros and cons.

### Pros:

  1. Better container isolation
  2. Better networking support
  3. Better performance
  4. No overhead managing a Docker host
  5. Great pricing (per minute and much lower pricing)
  6. User friendly tooling in the portal, including log support and running commands on containers using docker exec.

### Cons:

  1. The API still isn&#8217;t fully supported so things like build and push don&#8217;t work. You can mostly work around this using a docker registry.
  2. Lack of a Docker Host precludes using some of the patterns that have emerged for logging, monitoring, and sharing data between containers.

## Summary

Docker is a game changer but it is far from ready for prime time. Triton is the best choice available today for running container based workloads in general, and for production Docker workloads specifically.

At <a href="http://codefresh.io/" target="_blank" rel="nofollow">Codefresh</a>, we&#8217;re working to give you the benefits of containers in your development workflow without the headache of actually keeping the ship afloat yourselves.  Sign up for our free <a href="https://beta.codefresh.io/" target="_blank" rel="nofollow">Beta</a> service to see how simple we can make it for you to run your code and/or Docker containers in a clean environment. Take a look at my <a href="http://www.yonahruss.com/how-to/code-fast-with-codefresh.html" target="_blank" rel="nofollow">getting started walkthrough</a> or contact me if you have any questions.