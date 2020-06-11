---
id: 363
title: How To Setup a Multi-Platform Website With Cedexis
date: 2016-01-13T04:31:08-07:00
author: Yonah Russ
layout: revision
guid: http://www.yonahruss.com/tips/360-autosave-v1.html
permalink: /tips/360-autosave-v1.html
---
<img class="aligncenter size-full wp-image-361" src="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/multiplatform.jpg" alt="multiplatform" width="698" height="400" srcset="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/multiplatform.jpg 698w, http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/multiplatform-300x172.jpg 300w" sizes="(max-width: 698px) 100vw, 698px" />

With the recent and ongoing DDOS attacks against Github, many sites hosted as Github Pages have been scrambling to find alternative hosting. I began writing this tutorial long before the attacks but the configuration you find here is exactly what you need to serve static content from multiple providers like Github Pages, Divshot, Amazon S3, etc.

In my <a href="http://www.yonahruss.com/architecture/unpacking-cedexis-creating-multi-cdn-and-multi-cloud-applications.html" target="_blank" rel="nofollow">previous article</a>, I introduced the major components of Cedexis and how they fit together to build great multi-provider solutions. If you haven’t read it, I suggest taking a quick look to get familiar with the concepts.

In this article, I&#8217;ll show you, step-by-step, how to build a robust multi-provider, multi-platform website using the following Cedexis components:

  * Radar- provides real-time user measurements of provider performance and availability.
  * Sonar- provides lightweight efficient service availability health-checks.
  * OpenMix- DNS traffic routing based on the data from Radar and Sonar.
  * Portal- UI for configuration and reporting.

I&#8217;ll also briefly cover some of the reports available in the portal.

## Basic Architecture

OpenMix applications all have the same basic architecture and the same basic building blocks. Here is a diagram for reference:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAIVAAAAJDI0YjhjYzQ3LTg4MDEtNDQzZi04NjdmLTBkNjFjMDQyYzI0MQ.png" alt="" width="519" height="372" data-loading-tracked="true" /> 

## Preparations

For the purpose of the tutorial, I’ve built a demo site using the <a href="http://aws.amazon.com/s3/" target="_blank">Amazon S3</a> and <a href="https://divshot.com/" target="_blank">Divshot</a> static web hosting services. I’ve already uploaded my content to both providers and made sure that they are responding.

Both of these services provide us a DNS hostname with which to access the sites.

  * S3 &#8211; <a href="http://donatemyfee.com.s3-website-eu-west-1.amazonaws.com/" target="_blank" rel="nofollow">donatemyfee.com.s3-website-eu-west-1.amazonaws.com</a>
  * Divshot &#8211; <a href="http://production.donatemyfee.divshot.io/" target="_blank" rel="nofollow">production.donatemyfee.divshot.io</a>

Amazon S3 is part of the standard community Radar measurements, but as a recently launched service, Divshot hasn’t made the list yet. By adding test objects, we can eventually enable private Radar measurements instead.

Download the test objects from here:

  * <a href="https://portal.cedexis.com/api/v2/config/radar.json/testobject.js" target="_blank">Small Javascript Timing Object</a>
  * <a href="https://portal.cedexis.com/api/v2/config/radar.json/testobject.js?large=true" target="_blank">Large Javascript Timing Object</a>

I’ve uploaded them to a cedexis directory inside my web root on Divshot.

## Configuring the Platforms

Platforms are, essentially, named data sets. Inside the OpenMix Application, we assign them to a service endpoint and the data they contain influences how Cedexis routes traffic. In addition, the platforms become dimensions by which we slice and dice data in the reports.

<p class="left">
  We need to define platforms for S3 and Divshot in Cedexis and connect each platform to their relevant data sources (Radar and Sonar).
</p>

<p class="left">
  <img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAM4AAAAJDliMjA0ZDRiLThhY2UtNDZlYS1hOWUyLWFlMzY5NTQwNDg2ZA.jpg" alt="" width="395" height="92" data-loading-tracked="true" />
</p>

<p class="left">
  <img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAMJAAAAJDdmMWQ3NWYxLTg4NjUtNDlmZi1iMjY1LWRiZDI2NGYyM2FiZg.jpg" alt="" width="394" height="108" data-loading-tracked="true" />
</p>

<p class="left">
  Log in to the Cedexis portal <a href="https://portal.cedexis.com/static/beta/login.html" target="_blank">here</a> and click on the Add Platform button in the Platforms section.
</p>

&nbsp;

&nbsp;

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAKlAAAAJDVhYWVhMTQxLTI4NjEtNGQxMS04MWNhLThjMzUyNDZmYWRiMg.jpg" alt="" width="387" height="360" data-loading-tracked="true" /> We’ll find the community platform for Amazon S3 under the Cloud Storage category. It means that S3 performance will be monitored automatically by Cedexis’ Community Radar. You can leave the defaults on this screen:

After clicking next, we’ll get the opportunity to set up custom Radar and Sonar settings for this platform. We want to enable Sonar to make sure there are no problems with our specific S3 bucket which community Radar might not catch.

We’ll enable Sonar polls every 60 seconds (default) and for the test URL, I’ve put the homepage of the site.

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAKLAAAAJGZhZTg5NDQ4LWZhZGUtNGU2OS1iZmJmLWZiZmExNTAzOGUyZQ.jpg" alt="" width="588" height="224" data-loading-tracked="true" /> We’ll save the platform and create another:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAALsAAAAJDc0NmJmYzg2LTEyOGEtNDk2MS04MjRmLTJlM2FkYjU4NzA5Yw.jpg" alt="" width="588" height="109" data-loading-tracked="true" /> 

Divshot is somewhere in between Cloud Storage and Cloud Compute. It’s really only hosting static content so I’ve chosen the Cloud Storage category, but there is no real difference from Cedexis’ perspective. If they eventually add Divshot to their community metrics, it might end up in a different category.<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAALhAAAAJGQ1NzRjMTJhLTY1MzktNGZhNi1hMGI3LTlkYzJmYmI3N2E1NA.jpg" alt="" width="382" height="357" data-loading-tracked="true" />

Since Divshot isn’t one of the pre-configured Cloud Storage platforms, choose the platform “Other”.

The report name is what will show up in Cedexis charts when you want to analyze the data from this platform.

The OpenMix alias is how OpenMix applications will refer to this platform. Notice that I’ve called it divshot_production. That is because Divshot provides multiple environments for development, staging, and QA. In the future, we may define platforms for other environments as well.

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAALmAAAAJDQ1NjcxNDk3LTE0ZTctNDk0OC1iYmNkLTBmZDI0MjI0MmQ5Ng.jpg" alt="" width="381" height="279" data-loading-tracked="true" /> 

Since there are no community Radar measurements for Divshot, we prepare private measurements of our own in the next step.

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAIfAAAAJGIxN2NiNjYxLTlhOTQtNGVmMS05N2QyLWYxMDZmOTI0ZGQ1NA.jpg" alt="" width="384" height="288" data-loading-tracked="true" /> We are going to add three types of probes using the test objects which we downloaded above.****  
**** 

First the HTTP Response Time URL (If you are serving your content over SSL, as you should, then you should choose the HTTPS Response Time URL probe, etc.) The HTTP Response Time probe should use the Small Javascript Timing Object.

Click Add probe at the bottom left of the dialog to add the next probes.

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAI0AAAAJGRiOWI2OTg5LWRiY2ItNDg0NC1hMTU4LWU3ZGQ3NTk4Mjg1MA.jpg" alt="" width="385" height="313" data-loading-tracked="true" /> 

In addition to the response time probe, we will add the Cold Start and Throughput probes to cover all our bases.

The Cold Start probe should also use the small test object.

The Throughput probe needs the large test object.

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAIRAAAAJDQ4NDQzYzhiLTY0ZDMtNGI5OC04YWNiLWZiOWE0NzQ2YWYyMQ.jpg" alt="" width="383" height="307" data-loading-tracked="true" /> Make sure the test objects are correct in the summary page before setting up Sonar.

Configure the Sonar settings for Divshot similarly to those from S3 with the exception of using the homepage from Divshot for the test URL. Then click ‘Complete’ to save the platform and we are done setting up platforms for now.<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAK0AAAAJGJiZWFlYmYwLTUxY2QtNGI5OC1hYWVmLTRmYTNkMzMxNGFlNg.jpg" alt="" width="588" height="297" data-loading-tracked="true" />

A little bit of information about platforms. A nice thing about platforms is that they are decoupled from the OpenMix applications. That means that you can re-use a platform across multiple OpenMix applications with completely different logic. It also means you can slice and dice your data using application and platform as separate dimensions.

For example, if we had applications running in multiple datacenters, we would be interested to know about the performance and availability of each data center across all our applications. Conversely, we would also want to know if a specific application performs better in one data center than another. Cedexis hands us this data on a silver platter.

## Our First OpenMix Application

Open the Application Configuration option under the OpenMix tab and click on the plus in the upper right corner to add a new application..

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAH-AAAAJGEzYmNlMDM0LTdlNmItNGIxYy1iZjFlLWRmODQ5ODgzOWZiNA.jpg" alt="" width="588" height="135" data-loading-tracked="true" /><img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAMBAAAAJDVmMDFkN2VhLWE5ZWMtNDBiYS04YmYxLWZjNzBmZjUwZmNkYQ.jpg" alt="" width="588" height="103" data-loading-tracked="true" /> 

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAN4AAAAJGUzZDYyNWQxLTNhZDMtNDYxNS1iYWUxLTI4Y2JiM2U5ZTMwMA.jpg" alt="" width="380" height="355" data-loading-tracked="true" /> We’re going to select the Optimal RTT quick app for the application type. This app will send traffic to the platform with the best response time according to the information Cedexis has on the user making the request.

Define the fallback host. Note that this does not have to be one of the defined platforms. This host will be used in case the application logic fails or there is a system issue within Cedexis. In this case, I trust S3 slightly more than Divshot so I’ll configure it as the fallback host.

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAJtAAAAJGFhYWQ4NTRiLWE3OWQtNGM0ZC04ODI0LTk5ZjFlODZiODJjZA.jpg" alt="" width="378" height="194" data-loading-tracked="true" /> In the second configuration step, I’ve left the default TTL of 20 seconds. This means that users should check every 20 seconds to see if a different provider should be used to return requests. Once Cedexis detects a problem, the maximum time for users to be directed to a different provider should be approximately the same as this value.

In my experience, 20 seconds is a good value to use. It is long enough that users can browse one or two pages of a site before doing any additional DNS lookups and it is short enough to react to shorter downtimes.

Increasing this value will result in fewer requests to Cedexis. To save money, consider automatically changing TTLs via <a href="http://developers.cedexis.com/#sdks" target="_blank" rel="nofollow">RESTful Web Services</a>. Use lower TTLs during peaks, where performance could be more erratic, and use longer TTLs during low traffic periods to save request costs.

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAL0AAAAJGRjZGQ5YWExLWY2MTAtNGY3ZC05MjRiLWI5YmRjMWU5YTkyZQ.jpg" alt="" width="375" height="267" data-loading-tracked="true" /> On the third configuration step, I’ve left all the defaults. The Optimal RTT quick app will filter out any platforms which have less than 80% availability before considering them as choices for sending traffic.

Depending on the quality of your services, you may decide to lower this number but you probably do not want it any higher. Why not eliminate any platform that isn’t reporting 100% available? The answer is that RUM measurements rely on the, sometimes poor quality, home networks of your users and, as a result, they can be extremely finicky. Expecting 100% availability from a high traffic service is unrealistic and leaving a threshold of 80% will help reduce thrashing and unwanted use of the fallback host.

Regarding eDNS, you pretty much always want this enabled since many people have switched to using public DNS resolvers like Google DNS instead of the resolvers provided by their ISPs.

Shared resolvers break assumptions made by traffic optimization solutions and the eDNS standard works around this problem, passing information about the request origin to the authoritative DNS servers. Cedexis has supported eDNS from the beginning but many services still don’t.

In the final step, we will configure the service endpoints for each of the platforms we defined.

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAKsAAAAJDY0NDg2Yzc1LTg3MjctNDg0Yy1hYTM1LTUxOTA1ZThjZDFkZQ.jpg" alt="" width="381" height="390" data-loading-tracked="true" /> 

In our case, we are just associating the hostname aliases that Amazon and Divshot gave us with the correct platform and it’s Radar/Sonar data.

In a more complicated setup, you might have a platform per region of a cloud and service endpoints with different aliases or CNAMEs across each region.

Pay attention that each platform in the application has an “Enabled” checkbox. This makes it easy to go into an application and temporarily stop sending traffic to a specific platform. It is very useful avoiding downtime in case of maintenance windows, migrations, or intermittent problems with a provider.

Choose the Add Platform button on the bottom left corner of the dialog to add the second platform, not the Complete button on the bottom right.

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAANTAAAAJGY5NWU2ZWEyLWFjNTUtNDdjOC04ODA3LTE0MmFkYmYxNGU2NA.jpg" alt="" width="388" height="350" data-loading-tracked="true" /> Define the Divshot platform like we did for S3 and click Complete.

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAM7AAAAJGU1OWMwMGYxLTgzMDAtNDE3Zi1iYTZkLTQzYmU0MmEyZjc2YQ.jpg" alt="" width="389" height="160" data-loading-tracked="true" /> You should get a success message with the CNAME for the Cedexis application alias. Click &#8220;Publish&#8221; to activate the OpenMix application right away.

Alternatively, clicking &#8220;Done&#8221; would leave the application configured but inactive. When editing applications, you will get a similar dialog. Leaving changes saved but unpublished can be a useful way to stage changes to be activated later with the push of a button.

## Building a Custom OpenMix Application

The application we just created will work, but it doesn’t take advantage of the Sonar data that we configured. To consider the Sonar metrics, we will create a_custom_ OpenMix app and by custom, I mean copy and paste the code from <a href="https://github.com/cedexis/openmixapplib" target="_blank" rel="nofollow">Cedexis’ GitHub repository</a>. If you’re squeamish about code, talk to your account manager and I’m sure he’ll be able to help you.

The specific code we are going to adapt can be found <a href="https://raw.githubusercontent.com/yruss972/openmixapplib/8f4b1396a5ace397188c9d6b53c7c70a29816006/apps-javascript/ortt+sonar/app.js" target="_blank">here</a> (Note: I’ve fixed the link to a specific revision of the repo to make sure the instructions match, but you might choose to take the latest revision.)

We only need to modify definitions at the beginning of the script:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAOhAAAAJGJlNmIyZmEwLTRlY2YtNGJiNy04NGY3LWM3OGI5ZWQ1ODJjNg.jpg" alt="" width="588" height="283" data-loading-tracked="true" /> 

Let’s create a new application using the new script. Then we can switch back and forth between them if we want. We’ll start by duplicating the original app. Expand the app’s row and click Duplicate.<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAJOAAAAJDMzMTcxMTAwLTk1OTUtNDI1ZC1iODBjLWFlMzlmNjE5MTc5OQ.jpg" alt="" width="588" height="125" data-loading-tracked="true" />

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAANfAAAAJDhhOTI2YzI5LWI4YTEtNDU0NC1iZjVkLTAyMDExMWM4NzkwOA.jpg" alt="" width="389" height="364" data-loading-tracked="true" /> Switch the application type to that of a Custom Javascript app, modify the name and description to reflect that we will use Sonar data and click next.

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAJjAAAAJDhkNDEwMTI2LTE1Y2EtNDA3MS1iMmFlLTdlNjRkZjNkOTVmZA.jpg" alt="" width="391" height="203" data-loading-tracked="true" /> Leave the fallback and TTL as is on the next screen.

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAJAAAAAJDRkMTRkNWFkLWExZjktNGY1MC04NmVkLTI0NjViOGE1MTVkYg.jpg" alt="" width="390" height="171" data-loading-tracked="true" /> 

In the third configuration step, we’ll be asked to upload our custom application.  
<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAKfAAAAJDk0YjJiMjVkLTZlOTktNDczNS1iNjNhLTRiMzE5OTA4NjMxNQ.jpg" alt="" width="390" height="171" data-loading-tracked="true" /> Choose the edited version of the script and click through to complete the process.

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAALFAAAAJGQwYjc0MDU1LTMxODAtNDAyYS1hNTRjLWZkOTY4MGQ1ZjE5ZA.jpg" alt="" width="391" height="145" data-loading-tracked="true" /> As before, publish the application to activate it.

## Adding Radar Support to our Service

At this point, Cedexis is already gathering availability data on both our platforms via Sonar. Since we used the community platform for S3, we also have performance data for that. To finish implementing private Radar for Divshot, we must include the Radar tag in our pages so our users start reporting on their performance.

<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAM_AAAAJGMyNzVkYTg4LWRkYjktNDExNi05YWMzLTg3NmI5MDY4ZjM5ZQ.jpg" alt="" width="386" height="327" data-loading-tracked="true" /> We get our custom JavaScript tag from the portal (Note: If you want to get really advanced, you can go into the tag configuration section and set some custom parameters to control the behavior of the Radar tag, for example, how many test to run on each page load, etc.)  
<img class="right alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAK7AAAAJDM1YjBhMDY2LTA4ZTItNDEwZS05MzNhLTJiZjY0OTA0ZTVlYg.png" alt="" width="384" height="153" data-loading-tracked="true" /> Copy the tag to your clipboard and add it in your pages on all platforms.

# Going Live

Before we go live, we should really test out the application with some manual DNS requests, disabling and enabling platforms, to see that the responses change, etc.

Once we’re satisfied, the last step is to change the DNS records to point at the CNAME of the OpenMix application that we want to use. I’ll set the DNS to point at our Sonar enabled application.

A useful service to check how our applications are working is<a href="https://www.whatsmydns.net/" target="_blank">https://www.whatsmydns.net/</a>. This will show how our application CNAMEs resolve from location around the world. For example, if I check the CNAME resolution for the application we just created, I get the following results:<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAIyAAAAJDJiNDdhYTgwLWMwMjMtNDA1Zi05NzU0LTU2ODY0YjI0MTIwNA.jpg" alt="" width="588" height="415" data-loading-tracked="true" />

By and large, the Western Hemisphere prefers Divshot while the Eastern Hemisphere prefers Amazon S3 in Europe. This is completely understandable. Interestingly, there are exceptions in both directions. For example, in this test, OpenMix refers the TEANET resolver from Italy to Divshot while the Level 3 resolver in New York is referred to Amazon S3 in Europe. If you refresh the test every so often, you will see that the routings change.

Since this demo site isn’t getting any live traffic, I’ve generated traffic to show off the reports. First the dashboard which gives you a quick overview of your Cedexis traffic on login. Here we show that the majority of the traffic came from North America, and a fair amount came from Europe as well. We also see that, for the most part, the traffic was split evenly between the two platforms.<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAANaAAAAJDg1MTk5MWRiLWU2ZTUtNDg0Ny1hMmRhLTM2YTA1YzgzZDY5NQ.png" alt="" width="588" height="311" data-loading-tracked="true" />

To examine how Cedexis is routing our traffic, we look at the OpenMix Decision Report. I’ve added a secondary dimension of ‘Platform’ to see how the decisions have been distributed. You see that sometimes Amazon S3 is preferred and other times Divshot.<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAMoAAAAJGI2OGZmM2NkLTZmZDUtNGIzMy05OTFkLTU4Y2I2Njk4MTVjNg.png" alt="" width="588" height="257" data-loading-tracked="true" />

To figure out why requests were routed one way or the other, we can drill down into the data using the other reports. First, let’s check the availability stats in the Radar Performance Report. For the sake of demonstration, I’ve drilled down into availability per continent. In Asia, we see shoddy availability from Divshot but Amazon S3 isn’t perfect either. Since we didn’t see much traffic from Asia, this probably didn’t affect the traffic distribution. Theoretically, a burst of Asian traffic would result in more traffic going to Amazon.<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAIhAAAAJDQ5YzVhZjIyLWM0NjUtNDUxNi1iZGZkLTcwMTY2MjRjNzYwMQ.png" alt="" width="588" height="253" data-loading-tracked="true" />

In Europe, Divshot generally showed better availability than Amazon, reporting 100% except for a brief outage.<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAN_AAAAJDBmOWFjMzBhLTU0ODQtNDc4My04ZGFlLTE2ZTJjM2FmMDA5OA.png" alt="" width="588" height="251" data-loading-tracked="true" />

In North America, we see a similar graph. As to be expected, the availability of Amazon S3 in Europe is lower and less stable in North America. Divshot shows 100% availability which is also expected.<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAOhAAAAJDkwOTk2ODJkLTY2NTctNDg3ZS05OWQ4LTQxOTdkOTVjOWYzMw.png" alt="" width="588" height="252" data-loading-tracked="true" />

It’s important to note that the statistics here are skewed because we are comparing our private platform, measured only by our users to the community data from S3. The community platform collects many more data points in comparison to our private platform and it’s almost impossible for it to show 100% availability. This is also why we chose an 80% availability threshold when we built the OpenMix Application.

Next let’s look at the performance reports for response times of each platform. With very little traffic from Asia, the private performance measurements for Divshot are pretty erratic. With more traffic, the graph should stabilize into something more meaningful.<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAALkAAAAJGJiNzAxZWE0LWFhN2MtNDMxNi05ZWY1LWIxMTdlNzk0ZTE4Yg.png" alt="" width="588" height="252" data-loading-tracked="true" />

The graph for Europe behaves as expected showing Amazon S3 outperforming Divshot consistently.<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAK-AAAAJGQ5YjdhOGEzLTNmNDMtNDhjNi04ODM3LWVlZmEzNDVkNmU0Ng.png" alt="" width="588" height="253" data-loading-tracked="true" />

The graph for North America also behaves as expected with Divshot consistently outperforming Amazon S3.So we’ve seen some basic data on how our traffic performs. Cedexis doesn’t stop there. We can also take a look at how our traffic could perform if we add a new provider. Let’s see how we could improve performance in North America by adding other community platforms to our graph.<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAIDAAAAJDBlMzZjMTVmLTAxNzgtNDcyZC05YzkwLWZjMGM4NTEyNjFmYg.png" alt="" width="588" height="279" data-loading-tracked="true" />

I’ve added Amazon S3 in US-East, which shows almost a 30ms advantage on Divshot, though our private measurement still need to be taken lightly with so little traffic behind them. Even better performance comes from Joyent US-East. Using Joyent will require us to do more server management but if we really care about performance, Cedexis shows that it will provide a major improvement.

## Summary

To recap, In this tutorial, I’ve demonstrated how to set up a basic OpenMix application to balance traffic between two providers. Balancing between multiple CDNs or implementing a Hybrid Datacenter+CDN architecture is just as easy.

Cedexis is a great solution for setting up a multi-provider infrastructure. With the ever-growing Radar community generating performance metrics from around the globe, Cedexis provides both real-time situational awareness for your services operational intelligence so you can make well-informed decisions for your business.