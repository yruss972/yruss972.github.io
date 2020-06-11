---
id: 182
title: 'Xlib: PuTTY X11 proxy: wrong authentication protocol attempted'
date: 2009-11-24T15:01:31-07:00
author: Yonah Russ
layout: revision
guid: http://www.yonahruss.com/2009/11/180-revision-2.html
permalink: /tips/180-revision-2.html
---
While setting up some developers with remote SmartSVN via X over SSH using Plink, I ran into the following error:

> **Xlib: PuTTY X11 proxy: wrong authentication protocol attempted**

SmartSVN couldn&#8217;t connect to the tunneled X server display. I was extremely confused since I&#8217;d been using X tunneling successfully with SecureCRT. After googling the error message a little bit, it seems that the part about the &#8220;wrong authentication protocol attempted&#8221; is  misleading. You could get this message for not having the right magic cookie on the client side, or for not having a cookie at all as was apparently my case.

In my case, the developers are being authenticated against Active Directory via Samba/Winbind. Their home directories are non-existant until the first time they login via SSH. When using XForwarding over SSH, the ssh daemon on the server usually handles setting the DISPLAY and authentication cookies but in my case, it was trying to set up the cookies before the user&#8217;s home directory was created.

With some more digging, I found the $HOME/.ssh/rc and /etc/ssh/sshrc files which allows you to replace the standard XForwarding process with you custom process. Paraphrased from the sshd man page:

> The primary purpose of <kbd>$HOME/.ssh/rc</kbd> is to run any initialization routines that might be needed before the user&#8217;s home directory becomes accessible; AFS is a particular example of such an environment&#8230;
> 
> If X11 forwarding is in use, it will receive the <tt>proto cookie</tt> pair in its standard input and <tt>DISPLAY</tt> in its environment. The script must call <kbd>xauth</kbd> because <kbd>sshd</kbd> will not run <kbd>xauth</kbd> automatically to add X11 cookies&#8230;
> 
> This file will probably contain some initialization code followed by something similar to:
> 
> <pre>if read proto cookie && [ -n "$DISPLAY" ]
then
  if [ `echo $DISPLAY | cut -c1-10`  =  'localhost:' ]
  then
    # X11UseLocalhost=yes
    echo add unix:`echo $DISPLAY |
    cut -c11-` $proto $cookie
  else
    # X11UseLocalhost=no
    echo add $DISPLAY $proto $cookie
  fi | xauth -q -
fi</pre>
> 
> If this file does not exist, <kbd>/etc/ssh/sshrc</kbd> is run, and if that does not exist, <kbd>xauth</kbd> is used to store the cookie&#8230;
> 
> <kbd>/etc/ssh/sshrc</kbd> : Similar to <kbd>$HOME/.ssh/rc</kbd>. This can be used to specify machine-specific login-time initializations globally.

I pretty much cut and paste the code from the man page with two caveats-

  1. I used the full path to the xauth binary in the second to last line.
  2. I added the process to create the user&#8217;s home directory before the xauth.

That done, Plink was able to setup the XForwarding tunnel without a problem. I still can&#8217;t explain why Plink failed in the first place while SecureCRT had no problems with having the home directories appear later in the login process.

_Additional Reading &#8211; X Protocol background:_

X is a client-server protocol. The client program connects to a DISPLAY (usually defined in the similarly named environment variable) which represents the server displaying the GUI. Technically the display can refer to an X server on the same machine, on a remote machine on the same LAN, or even a server located across the Internet.

In order for a client to successfully connect to a display, the client needs to be authorized using either host authentication, cookie authentication, or user authentication. Host authentication allows all connections to an X server from one or more hosts/ip addresses. This is extremely insecure and should not be used. User authentication requires the client to authenticate as a user (using Kerberos for example) with authorization to access the X server. The most common authentication used with X servers is cookie authentication which basically uses a pre-shared key to authenticate clients. If your client knows the key, it gets in. If not, not.

In most cases, ie. every Linux desktop installation, the X server and client are on the same machine so both the server and client can easily look at the cookie in the user&#8217;s home directory. In the case of a remote connection (a purely X protocol connection between client and server over the network), the user will have to copy the cookie from the server side to the client side using the xauth utilities. Since the advent of SSH and XForwarding, this process has pretty much gone to pasture. The ssh client and ssh daemon are now mostly responsible for setting up authentication on the tunneled X connection.