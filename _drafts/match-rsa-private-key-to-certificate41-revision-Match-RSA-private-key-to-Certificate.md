---
id: 104
title: Match RSA private key to Certificate
date: 2007-06-16T22:57:00-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2007/06/41-revision.html
permalink: /uncategorized/41-revision.html
---
== Step One ==  
Run the following command on the key file to determine the modulus:  
`openssl rsa -noout -text -in secure.server.com.pem`

=== Example Output ===

<pre>Private-Key: (1024 bit)<br />modulus:<br />    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />    00:00:00:00:00:00:00:00:00<br />    ...<br /></pre>

== Step Two ==  
Run the following command on the certificate file to match the modulus:  
`openssl x509 -noout -text -in secure.server.com.pem`

=== Example Output ===

<pre>...<br />            RSA Public Key: (1024 bit)<br />                Modulus (1024 bit):<br />                    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />                    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />                    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />                    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />                    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />                    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />                    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />                    00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:<br />                    00:00:00:00:00:00:00:00:00<br />    ...<br /></pre>

If they match the certificate and key match.