---
id: 36
title: Redundant power for single power supply
date: 2007-02-15T12:40:00-07:00
author: Yonah Russ
layout: post
guid: http://yonahruss.com/wordpress/2007/02/redundant-power-for-single-power-supply.html
permalink: /architecture/redundant-power-for-single-power-supply.html
blogger_blog:
  - tempyonahruss.blogspot.com
blogger_permalink:
  - /2007/02/redundant-power-for-single-power-supply.html
dsq_thread_id:
  - "527051113"
categories:
  - Architecture
  - Tips
tags:
  - Emergency power system
  - high availability
  - Network switch
  - Power supply
  - rack
  - Railroad switch
  - redundant power
  - Switches
  - sysadmin
  - Technology_Internet
  - Transfer switch
---
Someone recently asked me how to setup redundant power for a rack of machines without relying on all the equipment to have dual power supplies.

The answer is called an Automatic Transfer Switch. Depending on the sensitivity of your equipment to momentary losses of power, you can choose between an Open Transition Transfer Switch and a Closed Transition Transfer Switch.

  * <span style="font-weight: bold;">Open Transition Transfer Switch </span>&#8211; This switch always makes the connection to the secondary power supply a split second after the first connection dies.
  * <span style="font-weight: bold;">Closed Transition Transfer Switch</span> &#8211; This switch will make the connection to the secondary power supply before the power from the first connection dies if possible, ie. the first connection is browning out, etc. In this case the switch briefly connects both power sources with an overlap time under 100 ms. This way there is never a complete loss of power.

Transfer Switches are not unreasonably priced considering the prices of an additional power supply for some servers but they do not solve single point of failure problems- If your single power supply fries, having a second feed won&#8217;t help it 😉