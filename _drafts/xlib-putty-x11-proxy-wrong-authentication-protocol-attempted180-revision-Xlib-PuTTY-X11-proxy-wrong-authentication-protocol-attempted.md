---
id: 181
title: 'Xlib: PuTTY X11 proxy: wrong authentication protocol attempted'
date: 2009-11-24T14:18:25-07:00
author: Yonah Russ
layout: revision
guid: http://www.yonahruss.com/2009/11/180-revision.html
permalink: /tips/180-revision.html
---
While setting up some developers with remote SmartSVN via X over SSH, I ran into the following error:

> **Xlib: PuTTY X11 proxy: wrong authentication protocol attempted**

SmartSVN couldn&#8217;t connect to the tunneled X server display. I was extremely confused sThis is a pretty cryptic error message and I was even more confused by the fact that I only had problems using Putty. Using SecureCRT,

_Quick Introduction:_

X is a client-server protocol. The client program connects to a DISPLAY (usually defined in the similarly named environment variable) which represents the server displaying the GUI. Technically the display can refer to an X server on the same machine, on a remote machine on the same LAN, or even a server located across the Internet.

In order for a client to successfully connect to a display, the client needs to be authorized using either host authentication, cookie authentication, or user authentication. Host authentication allows all connections to an X server from one or more hosts/ip addresses. This is extremely insecure and should not be used. User authentication requires the client to authenticate as a user (using Kerberos for example) with authorization to access the X server. The most common authentication used with X servers is cookie authentication.