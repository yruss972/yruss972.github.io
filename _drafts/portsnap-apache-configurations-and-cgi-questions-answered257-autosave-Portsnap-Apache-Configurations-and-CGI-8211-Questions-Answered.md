---
id: 259
title: 'Portsnap, Apache Configurations, and CGI &#8211; Questions Answered'
date: 2010-07-26T00:02:36-07:00
author: Yonah Russ
layout: revision
guid: http://www.yonahruss.com/tips/257-autosave.html
permalink: /tips/257-autosave.html
---
  1. Explain the importance of installing and running portsnap after installing a current version of FreeBSD. <p style="padding-left: 30px;">
      Portsnap is a system for securely distributing the FreeBSD ports tree. Approximately once an hour, a “snapshot” of the ports tree is generated, repackaged, and cryptographically signed. The resulting files are then distributed via HTTP.
    </p>
    
    <p style="padding-left: 30px;">
      The first time portsnap is run, it will need to download a compressed snapshot of the entire ports tree (portsnap fetch) and then a “live” copy of the ports tree can be extracted into /usr/ports/ (portsnap extract). This is necessary even if a ports tree has already been created in that directory (e.g., by using CVSup), since it establishes a baseline from which portsnap can determine which parts of the ports tree need to be updated later.
    </p>
    
    <p style="padding-left: 30px;">
      Initializing portsnap as soon as possible will ensure the most secure and up to date software installations on your machine and will prevent a long download of the initial compressed tree when you need it later.
    </p>
    
    <p style="padding-left: 30px;">
      After the initialization of portsnap, it is recommended to put &#8216;portsnap cron&#8217; in a cronjob to fetch updates regularly. Then you should use &#8216;portsnap update&#8217; before using the ports system. Putting &#8216;portsnap update&#8217; in cron is not recommended since it can cause problems if run while using the ports tree.
    </p>

  2. Explain the role configuration files in Unix applications. In Apache version 2.2, the configuration files have been modularized. What are the advantages and disadvantages of using a modular approach to configuration files? <p style="padding-left: 30px;">
      Configuration files allow you to control the settings and parameters of a service or program by editing in most cases a simple text file. Well known configuration files include /etc/hosts (local hostname to ip resolution), /etc/nsswitch.conf (name service configuration), /etc/resolv.conf (DNS server configuration). Samba uses a configuration file which looks more like a windows .ini file. Apache uses it&#8217;s own XML-ish format
    </p>
    
    <p style="padding-left: 30px;">
      Apache&#8217;s use of modular configuration files is not new in version 2.2 as can be seen here: http://httpd.apache.org/docs/1.3/mod/core.html#include
    </p>
    
    <p style="padding-left: 30px;">
      More likely you are used to a binary distribution of Apache which has split the configuration file into several files/directories and included them for te base configuration. The reason for doing this is convenience. It is easy to manage multiple Apache servers with similar settings by creating a basic shared configuration for all servers and only modifying a subset of the configuration sitting in an included file.
    </p>
    
    <p style="padding-left: 30px;">
      It is also popular to use a set of prepared configuration files (module configurations/vhost configurations) in a directory marked &#8220;_available&#8221; and symlink them into a directory called &#8220;_enabled&#8221; which is included in the Apache configuration. This provides a quick on/off mechanism for certain configurations.
    </p>

  3. Explain the use of “directives” in configuration files. Provide an example of two directives found in an Apache configuration file and detail what each accomplishes. <p style="padding-left: 30px;">
      Directives is a word which is pretty specific to Apache. Each directive controls some part of the configuration. Apache has ~410 of them. Each one is characterized by the syntax of the arguments it accepts, the default value if there is one, the context in which it can be used (server, virtual host, directory, etc.), what overrides must be in place for the directive to be used in a .htaccess file, status, module, and compatibility.
    </p>
    
    <p style="padding-left: 30px;">
      Examples:
    </p>
    
    <p style="padding-left: 60px;">
      ServerName is a directive which sets the request scheme, hostname and port that the server uses to identify itself. http://httpd.apache.org/docs/2.2/mod/core.html#servername
    </p>
    
    <p style="padding-left: 60px;">
      The ServerAdmin directive sets the contact address that the server includes in any error messages it returns to the client. http://httpd.apache.org/docs/2.2/mod/core.html#serveradmin
    </p>

  4. What is meant by “overrides”? Provide an example of an override found in an Apache configuration file and detail what is accomplished by the override. <p style="padding-left: 30px;">
      An override allows a directive to be overridden by directives in a .htaccess file located in one of the web content directories.
    </p>
    
    <p style="padding-left: 30px;">
      An example of an override is AuthConfig. This override will allow the .htaccess file in a directory to change the apache configuration of that directory in terms of authentication (either allow or deny access, specify users, etc.) http://httpd.apache.org/docs/2.2/mod/core.html#allowoverride
    </p>

  5. Define what is meant up a Common Gateway Interface, how it is used in websites, and the methods for providing one? Discuss the advantages and disadvantages of providing this functionality on a web site. <p style="padding-left: 30px;">
      CGI is an older standard for allowing a web server like Apache to send request parameters to an external program and use the program&#8217;s output as a response. Before scripting languages like PHP or PERL where built straight into web server modules, this was the only way to use dynamically generated content.
    </p>
    
    <p style="padding-left: 30px;">
      CGI is generally not a great solution today although it is still used. It&#8217;s performance is poor due to the need to start a completely new process on each request. CGIs generally take more memory and require more open processes on the server. PERL has a CGI module which makes writing a CGI script fairly easy.
    </p>
    
    <p style="padding-left: 30px;">
      More common today is the use of FastCGI which requires a different interface from the external program. FastCGI keeps a number of external programs running to improve performance. Many recommend running PHP as a FastCGI program in order to take advantage of Apache&#8217;s newer multi-threaded MPM.
    </p>