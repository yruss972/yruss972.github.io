---
id: 100
title: 'Solaris 10 doesn&#8217;t find network card'
date: 2007-03-01T07:03:00-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2007/03/38-revision.html
permalink: /uncategorized/38-revision.html
---
I recently installed Solaris 10 06/06 x86 on my desktop machine, a Compaq Evo with an onboard Intel 10/100 network card.

At first the Solaris installation seemed to hang while trying to find a network configuration from a non-existant RPC boot server. In retrospect, I think the problem was that Solaris didn&#8217;t find an appropriate driver for the card but after waiting a long time, the installation continued skipping the network configuration.

Running <span style="font-weight: bold;">prtconf -pv </span>shows the pci identification details for the ethernet card:

> model: &#8216;Ethernet controller&#8217;  
> power-consumption: 00000001.00000001  
> fast-back-to-back:  
> devsel-speed: 00000001  
> interrupts: 00000001  
> max-latency: 00000038  
> min-grant: 00000008  
> subsystem-vendor-id: 00000e11  
> subsystem-id: 00000012  
> unit-address: &#8216;8&#8217;  
> class-code: 00020000  
> revision-id: 00000081  
> <span style="font-weight: bold;">vendor-id: 00008086</span>  
> <span style="font-weight: bold;">device-id: 0000103b</span>  
> name: &#8216;pcie11,12&#8217;

Looking up the identification information in the [PCI ID repository](http://pci-ids.ucw.cz/iii//?i=8086103b) tells me I&#8217;m dealing with a 82801DB PRO/100 VM (LOM) Ethernet Controller

Looking at /boot/solaris/devicedb/master, I found the following similar drivers:

> bash-3.00# grep 82801DB /boot/solaris/devicedb/master  
> pci8086,1039 pci8086,1039 net pci iprb.bef &#8220;Intel 82801DB Ethernet 82562ET/EZ PHY&#8221;  
> pci8086,103d pci8086,103d net pci iprb.bef &#8220;Intel 82801DB PRO/100 VE Ethernet&#8221;

Both cards use the iprb driver so I add the identifier for my driver into /etc/driver_aliases:

> iprb &#8220;pci8086,1038&#8221;  
> iprb &#8220;pci8086,1039&#8221;  
> <span style="font-weight: bold;">iprb &#8220;pci8086,103b&#8221;</span>  
> iprb &#8220;pci8086,103d&#8221;

Load the driver with the modload command and plumb the interface:

> modload /kernel/drv/iprb  
> ifconfig iprb0 plumb

If that works, create the /etc/hostname.iprb0 file. I wanted to use DHCP so I did the following:

> touch /etc/dhcp.iprb0  
> touch /etc/hostname.iprb0

Then do a reconfigure reboot.