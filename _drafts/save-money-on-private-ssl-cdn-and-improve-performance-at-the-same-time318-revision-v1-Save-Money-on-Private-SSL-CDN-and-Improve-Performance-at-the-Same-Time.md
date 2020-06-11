---
id: 320
title: Save Money on Private SSL CDN and Improve Performance at the Same Time
date: 2016-01-12T08:15:40-07:00
author: Yonah Russ
layout: revision
guid: http://www.yonahruss.com/tips/318-revision-v1.html
permalink: /tips/318-revision-v1.html
---
<img class="aligncenter size-full wp-image-319" src="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/burning.jpg" alt="burning money" width="698" height="400" srcset="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/burning.jpg 698w, http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/burning-300x172.jpg 300w" sizes="(max-width: 698px) 100vw, 698px" />

SSL support in your site, and therefore, in your CDN is critical but it is also incredibly expensive. In this article, I&#8217;ll show you how to save a fortune while improving the performance and security of your site.

It used to be that sites only encrypted the most sensitive traffic with their customers, i.e. registration, login, checkout, etc. In 2010, the <a href="https://en.wikipedia.org/wiki/Firesheep" target="_blank" rel="nofollow">Firesheep</a> extension made it very clear that this is not enough.

Since then, many other attacks on partially encrypted sites have been devised and so major sites like Google, Twitter, Facebook, and others have all switched to HTTPS by default, aka <a href="https://otalliance.org/resources/always-ssl-aossl" target="_blank" rel="nofollow">Always On SSL</a>. Last year, Google called for <a href="https://www.youtube.com/watch?v=cBhZ6S0PFCY" target="_blank" rel="nofollow">HTTPS everywhere</a> and later, announced that secured sites will get <a href="http://googleonlinesecurity.blogspot.co.il/2014/08/https-as-ranking-signal_6.html" target="_blank" rel="nofollow">boosts in PageRank</a>.

## Shared SSL from the CDN is cheap.

Shared SSL in the CDN is better than nothing but it isn&#8217;t great. It&#8217;s better than nothing because it will help with <a href="https://community.qualys.com/blogs/securitylabs/2014/03/19/https-mixed-content-still-the-easiest-way-to-break-ssl" target="_blank" rel="nofollow">mixed content warnings</a>. It&#8217;s not great because it isn&#8217;t on the same domain as your site so it can cause <a href="https://en.wikipedia.org/wiki/Same-origin_policy" target="_blank" rel="nofollow">same origin policy</a> problems.

## Why is Private SSL CDN so expensive?

That&#8217;s a great question and I&#8217;m not sure there is a good answer. Some of the common excuses are:

  1. Encrypted traffic is heavier than unencrypted traffic so it should cost more. This is only very slightly true and recent work in this area has made this even less true.
  2. The logistics of deploying the SSL Certificates across all the CDN edge nodes is expensive. It&#8217;s honestly hard to believe that companies base all their business on managing uncountable edge nodes, have real trouble deploying the certificates.
  3. It&#8217;s only worth it for the CDN company to assume the security risk of HTTPS traffic if they charge you more money. This is probably the closest answer to the truth.

Regardless of the real reason, CDN&#8217;s often charge both exorbitant one time setup fees and high monthly fees for each SSL enabled configuration.

## Common mistakes which are costing you money.

Companies often make several mistakes when ordering SSL services, costing them thousands of extra dollars each month:

  1. Ordering too many domains
  2. Ordering the wrong type of SSL service
  3. Not negotiating the fees

### Ordering too many domains

Companies often spread their sites over multiple subdomains, i.e. www.yahu.com<a href="http://www.yahoo.com%2C/" target="_blank">,</a> mail.yahu.com, shopping.yahu.com, etc. Each of these sites will have it&#8217;s own content and many companies understandably setup separate CDN configurations for each origin, i.e. cdn.www.yahu.com, cdn.mail.yahu.com.

Ordering SSL from the CDN for each of those domains will cost a fortune. Instead, create a single CDN configuration for each root domain, i.e. cdn.yahu.com, cdn.giggle.com. In each configuration, use the CDN provider&#8217;s built in URL rewriting and origin rewriting features to direct the requests to the appropriate origin.

For example, configure cdn.yahu.com/www/ to cache content from www.yahu.com while cdn.yahu.com/shopping/ caches content from shopping.yahu.com. Now you have cut down the number of SSL slots you need to order drastically.

### Ordering the wrong type of SSL service

Different CDN providers offer different types of SSL service. Some provide standard single domain certificates. Some use a multi-domain certificate. Some offer wildcard certificates.

Now that you have minimized the number of domains you need to protect in the CDN, you can re-evaluate the type of SSL certificate you need. In the example above, we might have thought to order an expensive wildcard certificate to protect all the subdomains of yahu.com, whereas now we can choose a less expensive single domain certificate.

If we can&#8217;t use URL rewriting to save on SSL enabled CDN domains, it may be less expensive to get one wildcard certificate, than to get several domains on a multi-domain certificate.

### Not negotiating the fees

CDN fees are _always_ negotiable. Usually, the more traffic you commit to each month, the lower a price you get. SSL slots are also open to volume discounts. If you need multiple SSL slots, try to order them at the same time and ask for a discount on the setup fees. The logic- even if they install each certificate manually, they can install all your certs at the same time.

You should also be able to get a discount on the monthly fees just because you are committing to pay them more each month. A better deal to ask for, is to offer to over-commit on monthly traffic commitment, in exchange for a discount on the SSL monthly fees.

For example, you have a monthly $5K commit for traffic and you are adding five SSL slots at a $750/month list price. They offer to go down to $700 monthly per SSL because you are adding five (a total of $8.5K monthly commitment). Counter back with an offer to commit to $6K monthly traffic + $500 monthly per SSL. This is better because, even if your traffic grows by 20%, you will continue paying $8.5K/month but you are still getting the SSL service.

## How does this improve performance?

There is an added, hidden benefit in consolidating your CDN hostnames.

Now, when a customer first reaches one of your sites, i.e. www.yahu.com, their browser looks up and connects to your consolidated CDN domain &#8211; cdn.yahu.com. When they browse your site and switch to a new subdomain, for example if they clicked on shopping.yahu.com, they are already connected to cdn.yahu.com. They save on additional DNS lookups and additional TCP handshakes (the most expensive part of SSL traffic).

You can push this even further by sharing resource between your origins using a special CDN location, for example by storing common CSS files at cdn.yahu.com/shared/. In this case, shopping.yahu.com will be able to use the pre-loaded and cached CSS files from the browser&#8217;s initial visit to www.yahu.com.

## Summary

I&#8217;ve used these SSL consolidation techniques in both <a href="http://www.akamai.com/" target="_blank" rel="nofollow">Akamai</a> and <a href="http://www.cdnetworks.com/" target="_blank" rel="nofollow">CDNetworks</a> realizing significant cost and performance savings. Although I&#8217;ve worked with at least five other CDN providers, I haven&#8217;t tried these techniques elsewhere.

If you&#8217;re interested in optimizing your CDN deployment for best cost/performance, feel free to contact me via <a href="https://il.linkedin.com/in/yonahruss" target="_blank" rel="nofollow">LinkedIn</a> or via <a href="https://donatemyfee.org/" target="_blank" rel="nofollow">https://donatemyfee.org/</a>.