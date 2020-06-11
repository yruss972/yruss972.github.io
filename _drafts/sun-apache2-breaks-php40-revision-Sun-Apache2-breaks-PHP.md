---
id: 105
title: Sun Apache2 breaks PHP?
date: 2007-05-13T05:06:00-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2007/05/40-revision.html
permalink: /uncategorized/40-revision.html
---
This post isn&#8217;t going to solve anyone&#8217;s problems but maybe someone will solve mine.  
I recently compiled and packaged a new release of php4 (4.4.7) for use with Sun&#8217;s Apache2 (Solaris 10 11/06 patched).  
Unfortunately for some very strange reason, Apache segfaults whenever it tries to server a page. It segfaults even if the page has no php involved and only if the php module is loaded.

Here is some sample truss output:

> ``
> 
> <pre>2146:   stat64("/var/apache2/vservers/htdocs/favicon.ico", 0xFFBFF720) Err#2 ENOENT<br />2146:   lstat64("/var", 0xFFBFF720)                     = 0<br />2146:   lstat64("/var/apache2", 0xFFBFF720)             = 0<br />2146:   lstat64("/var/apache2/vservers", 0xFFBFF720)    = 0<br />2146:   lstat64("/var/apache2/vservers/htdocs", 0xFFBFF720) = 0<br />2146:   lstat64("/var/apache2/vservers/htdocs/favicon.ico", 0xFFBFF720) Err#2 ENOENT<br />2146:       Incurred fault #6, FLTBOUNDS  %pc = 0xFE887CF8<br />2146:         siginfo: SIGSEGV SEGV_MAPERR addr=0x00000054<br />2146:       Received signal #11, SIGSEGV [caught]<br />2146:         siginfo: SIGSEGV SEGV_MAPERR addr=0x00000054<br />2146:   schedctl()                                      = 0xFEE06000<br />2146:   lwp_sigmask(SIG_SETMASK, 0x00000400, 0x00000000) = 0xFFBFFEFF [0x0000FFFF]<br />2146:   chdir("/usr/apache2")                           = 0<br />2146:   sigaction(SIGSEGV, 0xFFBFF2B8, 0xFFBFF358)      = 0<br />2146:   getpid()                                        = 2146 [912]<br />2146:   getpid()                                        = 2146 [912]<br />2146:   kill(2146, SIGSEGV)                             = 0<br />2146:   setcontext(0xFFBFF298)<br />2146:       Received signal #11, SIGSEGV [default]<br />2146:         siginfo: SIGSEGV pid=2146 uid=80</pre>

I configured Solaris to dump core and gdb&#8217;d the core file:

> ``
> 
> <pre>#0  php_handler (r=0x1964e8) at /root/dev/php-4.4.7/sapi/apache2handler/sapi_apache2.c:470<br />470     /root/dev/php-4.4.7/sapi/apache2handler/sapi_apache2.c: No such file or directory.<br />       in /root/dev/php-4.4.7/sapi/apache2handler/sapi_apache2.c<br />(gdb) bt full<br />#0  php_handler (r=0x1964e8) at /root/dev/php-4.4.7/sapi/apache2handler/sapi_apache2.c:470<br />       ctx = (php_struct *) 0x0<br />       conf = (void *) 0x0<br />       brigade = (apr_bucket_brigade *) 0x197f38<br />       bucket = (apr_bucket *) 0x0<br />       parent_req = (request_rec *) 0x0<br />#1  0x0002e730 in ap_run_handler ()<br />No symbol table info available.<br />#2  0x0002ed20 in ap_invoke_handler ()<br />No symbol table info available.<br />#3  0x0002baa8 in ap_process_request ()<br />No symbol table info available.<br />#4  0x0002670c in .st_double_foreff ()<br />No symbol table info available.<br />#5  0x0002670c in .st_double_foreff ()<br />No symbol table info available.<br />Previous frame identical to this frame (corrupt stack?)<br />(gdb) quit</pre>

I found some people with a similar problem but no answers here: http://forum.java.sun.com/thread.jspa?threadID=5137425&tstart=270

I even tried my old package for php 4.4.3 but no luck. The only difference between the old machine and the new that I know of is that the old machine is 06/06 and the new one is 11/06????

In the end, I ditch Sun&#8217;s Apache2 and install Blastwave packages and everything works.

All I can say is that Sun is wasting it&#8217;s time on desktops. Sun needs to bring their software delivery and upgrade management up to Redhat/Debian standards. If they would just do that, they could start charging for their OS again.