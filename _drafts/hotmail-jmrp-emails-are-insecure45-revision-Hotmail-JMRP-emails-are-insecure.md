---
id: 103
title: Hotmail JMRP emails are insecure!
date: 2007-07-15T23:16:00-07:00
author: Yonah Russ
layout: revision
guid: http://yonahruss.com/wordpress/2007/07/45-revision.html
permalink: /uncategorized/45-revision.html
---
For those who aren&#8217;t familiar, Microsoft has a program called JMRP (Junk Mail Reporting Program). Mail senders can sign an agreement with Microsoft to receive complaints about mail sent from their domains. Basically that means, if I send email to your Hotmail account, and you complain that it is spam, Microsoft will forward the email back to a specific email address so I can unsubscribe you, or otherwise handle the complaint.

Where is the security issue? The emails Microsoft sends are not signed in any way. There is no way to verify that the email was actually sent by Microsoft, nor can you be sure that the content was not altered. Basically, anyone who finds your company&#8217;s JMRP address, can send false complaints and at the very least tie up your customer service people with wild goose chases.

Automated systems for handling these emails can only verify the SPF records and hope that the headers haven&#8217;t been spoofed or messages altered in transit (Unlike Domain Keys or DKIM, SenderID doesn&#8217;t protect against such issues.) The possible attacks are two fold-

  1. Forge complaints from known subscribers who want to receive the mailings, thereby causing the sender to lose business
  2. Alter real complaints in transit so that complainers don&#8217;t get unsubscribed, thereby hurting the mail sender&#8217;s reputation.

This could all be solved just by signing the emails using S/MIME or some other form of validation but Microsoft doesn&#8217;t seem to be interested.

In short, don&#8217;t trust JMRP emails blindly. Tell Microsoft you want them to sign the emails so we can all receive less spam.