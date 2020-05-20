---
id: 303
title: How to Host a Screaming Fast Site for $0.03/Month
date: 2014-12-31T07:44:15-07:00
author: Yonah Russ
layout: post
guid: http://www.yonahruss.com/?p=303
permalink: /architecture/how-to-host-a-screaming-fast-site-for-0-03month.html
dsq_thread_id:
  - "4484588818"
categories:
  - Architecture
  - How To
  - Performance
  - Tips
tags:
  - Ajax
  - Amazon
  - Amazon CloudFront
  - Amazon S3
  - Amazon Web Services
  - basic solution
  - CloudFlare
  - Content delivery network
  - DNS
  - File hosting
  - Firewall
  - free online services
  - google
  - HTML
  - HTTP
  - JavaScript
  - JavaScript libraries
  - JQuery
  - LinkedIn
  - load balancing
  - Minification
  - normal infrastructure
  - responsive web design techniques
  - social networks
  - static web
  - Technology_Internet
  - user on-site
  - web performance
  - Web services
  - web serving infrastructure
---
<img class="aligncenter size-full wp-image-304" src="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/perf.jpg" alt="perf" width="696" height="399" srcset="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/perf.jpg 696w, http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/perf-300x172.jpg 300w" sizes="(max-width: 696px) 100vw, 696px" />

I had an idea. That&#8217;s always how it starts. Before I know it, I&#8217;ve purchased the domain name and I&#8217;m futzing around with some HTML but where am I going to host it and how much is this going to end up costing me?

That&#8217;s where I was when I came up with <a href="http://donatemyfee.com/" target="_blank" rel="nofollow">#DonateMyFee</a>. &#8220;This is a site that is only going to cost me money&#8221;, I thought to myself (the whole point is for people to donate money rather than paying me). I really didn&#8217;t want to start shelling out big (or small) bucks on hosting.

Long story short, here is the recipe for a screaming fast website on a low budget:

  * <a href="https://aws.amazon.com/s3/" target="_blank" rel="nofollow">Amazon S3</a> for cheap static web hosting
  * <a href="http://www.cloudflare.com/" target="_blank" rel="nofollow">CloudFlare </a>for free DNS and CDN
  * Frugal functionality, including free form processing a la <a href="http://www.google.com/forms/about/" target="_blank" rel="nofollow">Google Forms</a>
  * Less is more

### Amazon S3

I&#8217;m not a huge fan of Amazon AWS, but S3 is useful enough to make it into my good graces. S3 is Amazon&#8217;s storage service. You upload static files into &#8220;buckets&#8221; and S3 can hold on to them, version them, and most importantly serve them via http. When configured to serve a bucket as a static website, S3 can be used to replace the load balancing and web serving infrastructure needed to serve a static website.

There are only two problems with that.

  1. You pay for S3 by the amount of traffic pulled from your bucket.
  2. Your &#8220;website&#8221; will be called something crazy ugly like <a href="http://donatemyfee.com.s3-website-eu-west-1.amazonaws.com/" target="_blank" rel="nofollow">donatemyfee.com.s3-website-eu-west-1.amazonaws.com</a>

Regarding the price, S3 tries to get you three ways. They charge for the volume of the data being stored, for the number of requests made, and for the volume of the request throughput in GB. That said, the prices are very reasonable if we can keep the number of requests low. For that reason, a CDN is an absolute must. The CDN will also solve our second problem &#8211; the unfriendly S3 website name.

Often S3 is paired with Amazon&#8217;s CDN, Cloudfront, but I don&#8217;t recommend it. Cloudfront is expensive as CDN&#8217;s go and we&#8217;re on a budget. Even if we wanted to pay for the CDN, there are better performing options for less. CloudFlare is a great alternative with a free plan that will do us wonders.

### CloudFlare

CloudFlare is one of several CDN by proxy + Webapp Firewall solutions that cropped up several years ago. Since the beginning, they have had a free plan and they have proven to be both innovative and competitive.

To use CloudFlare , we need to set their servers as your domain&#8217;s DNS name servers which can be a deal breaker in some cases. Once that&#8217;s setup we create a CNAME record in CloudFlare which points to the ugly S3 website name. CloudFlare has a new CNAME flattening technique which will allow us to configure this even for the root domain (without the www). This technique break some rules so I wouldn&#8217;t recommend it in every case, but in ours, it&#8217;s just what we need.

CloudFlare will cache all of our static content from S3 saving us from paying for the majority of the visits to the site. CloudFlare will also compress and optimize our content so it takes less time to reach the browser. Depending on what kind of traffic your site attracts, CloudFlare&#8217;s security settings can also protect you from all kinds of resource abuse, malicious traffic, hotlinking, etc.

_Note: S3 will not properly identify the mime types for every file which means that some files might not be compressed properly by CloudFlare. You can fix this by changing the metadata for the files in S3. Specifically .ttf, .eot, and other typography related files are a problem._

### Frugal Functionality

Having a cheaply hosted static website is nice but static can also be pretty useless. In order to get some functionality out of the site, you could go all jQuery on it but I that that is a road too often traveled these days. I&#8217;ve seen too many people include all of jQuery instead of writing 3 lines of JavaScript.

If we want this site to be fast we need to work frugally. If you take a look at<a href="http://donatemyfee.com/" target="_blank">http://donatemyfee.com</a>, you will see some examples of what I call &#8220;frugal functionality&#8221;.

The social share buttons are static links, not huge JavaScript widgets included from various social networks. Including external scripts is always a bad idea and they always hurt the performance of your site no matter what anyone tells you. Also, the icons and hover animations are <a href="http://codepen.io/markmurray/pen/JugrG" target="_blank" rel="nofollow">CSS typography tricks</a>. No JavaScript and no icon images downloaded.

The site is designed using responsive web design techniques which is &#8220;buzzword&#8221; for using a bunch of <a href="https://github.com/tylerchilds/Vanilla-HTML" target="_blank" rel="nofollow">crafty CSS</a> to make the same thing look decent on different sized screens. If we were a large company, I would say &#8220;Responsive web is for lazy companies and people without a budget to develop good looking, device targeted sites.&#8221; Since we&#8217;re on a budget, I&#8217;ll say it&#8217;s frugal 🙂

Last but not least, we have skimped on all the normal infrastructure that goes behind a website so our options for actually generating leads are a bit thin. We could go very old school with <a href="https://en.wikipedia.org/wiki/Mailto" target="_blank" rel="nofollow">mailto</a> links but in these days where webmail reigns supreme, they are getting pretty useless. Enter Google Forms.

### Google Forms

If you haven&#8217;t been asked to fill out a Google Form yet, <a href="http://goo.gl/forms/pnYUWGOc4P" target="_blank" rel="nofollow">here&#8217;s</a> your chance. Google lets you create fairly elaborate forms for free. The forms collect the answers and store them automatically in a Google Drive spreadsheet. There are more sophisticated options for processing the answers, and an entire extension ecosystem being built around the process. For us, the basic solution is more than enough.

Note: You can link to the form or embed it in an iframe. The form will take a bite out of your page load performance (iframes are a huge performance no-no). They will also annoy you with endless warnings, all of which you can nothing about, if you test your site performance with any of the free online services (<a href="http://www.webpagetest.org/" target="_blank" rel="nofollow">Webpagetest</a>,<a href="http://www.websitetest.com/" target="_blank" rel="nofollow">Websitetest</a>, <a href="http://gtmetrix.com/" target="_blank" rel="nofollow">GTmetrix</a>, <a href="https://developers.google.com/speed/pagespeed/insights/" target="_blank" rel="nofollow">PageSpeed</a>, etc.). In this case, I used some simple (read jQuery-free) JavaScript to load the embeded iframe if it&#8217;s requested. This has the added benefit of keeping the user on-site to fill out the form and eliminating the page load time performance hit.

### Less is more

Finally, the most important advice about web performance is always &#8220;Less is more&#8221;. There is no better way to ensure that a page loads quickly than to make it smaller in every way possible. Use less and smaller pictures. Combine, compress and minify everything. Reduce the number of requests.

_If you&#8217;re interested in getting my help with your site, contact me via <a href="http://il.linkedin.com/in/yonahruss/" target="_blank" rel="nofollow">LinkedIn</a> or<a href="http://donatemyfee.com/" target="_blank" rel="nofollow">#DonateMyFee</a> . All consulting fees go directly from you to a tax deductible charity in your/your company&#8217;s name._