---
id: 365
title: Code Fast with CodeFresh!
date: 2015-06-10T04:09:52-07:00
author: Yonah Russ
layout: post
guid: http://www.yonahruss.com/?p=365
permalink: /how-to/code-fast-with-codefresh.html
categories:
  - How To
tags:
  - CD
  - CI
  - Codefresh
  - Continuous Deployment
  - Continuous Integration
  - Docker
  - Fast Feedback
  - Github
---
<img class="aligncenter size-full wp-image-366" src="/assets/images/2016/01/cf.jpg" alt="cf" width="698" height="400" srcset="/assets/images/2016/01/cf.jpg 698w, /assets/images/2016/01/cf-300x172.jpg 300w" sizes="(max-width: 698px) 100vw, 698px" />

In case you were wondering why I haven&#8217;t written a word in the past two months, the answer is <a href="http://codefresh.io/" target="_blank" rel="nofollow">CodeFresh</a>. We&#8217;ve been working day and night with our private beta customers to make CodeFresh the fastest and easiest way for you to hack on your code.

## What is CodeFresh?

It&#8217;s super simple. It&#8217;s super fast. It&#8217;s easiest to explain with a quick walkthrough:

First, go to <a href="https://beta.codefresh.io/" target="_blank">https://beta.codefresh.io/</a> and login with your Github or Bitbucket account:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAK8AAAAJDAxM2U0ZjQ2LTU1MDQtNGQ1MS05OGI3LWQzZmVjMzgyMTYzZg.jpg" alt="" width="588" height="326" data-loading-tracked="true" /> Once you&#8217;ve authorized the application you&#8217;ll get a list of your repositories. <img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAMyAAAAJDZkYzMxMzMyLWU0YTctNDhlMS05YWI0LWFiNjIzNWQ4MDc2Mw.jpg" alt="" width="588" height="369" data-loading-tracked="true" />

If you&#8217;re part of an organization on Github, you can switch to the organizational context in the upper right corner:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAALmAAAAJDdkODBiMGRmLWJjMzQtNDExMC04ZTFlLTRhZjQ3ZWQ4MTc1Mw.jpg" alt="" width="588" height="195" data-loading-tracked="true" /> I want to hack on the math-race project so I click &#8220;Launch&#8221;:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAALqAAAAJDcxZjY5Y2Q3LWI0M2ItNDQ3ZS04MTZlLTVkNWVmMGJhNWI5YQ.jpg" alt="" width="588" height="206" data-loading-tracked="true" /> A div will pop up showing me the progress as CodeFresh clones the repo and automatically builds a Docker container with my app inside it. At the end of the process I get two green buttons &#8211; &#8216;Open in IDE&#8217; and &#8216;Launch app&#8217;.

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAMzAAAAJGY1NGVkMThlLTNiMWItNGFjZS04YzNhLTY0NTJlMDAzNDZiNA.jpg" alt="" width="588" height="615" data-loading-tracked="true" /> Let&#8217;s start with &#8216;Launch app&#8217;:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAIPAAAAJGEwMTI0YTc4LWZjYzYtNDNlOS04YjI1LTE2OWE3ZjczN2IyMw.jpg" alt="" width="588" height="263" data-loading-tracked="true" /> The browser opens up a new tab with my app which is already running but there is actually a problem (yes, I knew there was going to be a problem). When I fill in the name field, the game is supposed to start.

Let&#8217;s go back and open up the IDE. This will put me into a custom version of the Orion web based IDE, including a shell window running in the Docker container:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAMTAAAAJDg0MDU3YmYwLWEwMDEtNDg0Yi1hZDhkLTE4M2MwOWMzODdiNA.jpg" alt="" width="588" height="325" data-loading-tracked="true" /> Specifically, I know that this app needs the host and port that the app is running on to be hard coded into the config.js file, so I&#8217;ll open that up in the IDE and give it the host and port from the tab the app launched in. Don&#8217;t forget to save the file.

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAKkAAAAJGE1ZGFhYTk4LWJiOTMtNDM1MS04NDcwLTc0NWU3MGRhNjVjMg.jpg" alt="" width="588" height="237" data-loading-tracked="true" /> Then click the stop and start buttons at the top of the IDE to re-launch the app.

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAKDAAAAJGUzOWQ0NGZiLTJmOWItNDI5MS04MmIzLTBiMTE1OWMxNTQ1ZQ.jpg" alt="" width="588" height="140" data-loading-tracked="true" /> Then I switch back to the other browser tab and hit refresh and everything is working:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAN4AAAAJGU2M2E0N2FkLTA4YmItNDI2ZC04ZmZlLTczOTNiYzYwMzVlYQ.jpg" alt="" width="588" height="286" data-loading-tracked="true" /> I could just as easily have edited the files in vi from the terminal:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAANUAAAAJDU1MTgyMGMxLTNiOTctNDY2MC05M2VkLTlkYmRmMDgwNmJmZg.jpg" alt="" width="588" height="208" data-loading-tracked="true" /> And once I&#8217;m done with my changes, I can commit them and push them back to git:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAALOAAAAJDIxNjczMzAwLWIyYTItNGM5My04NzRmLWMzN2Q4YmFkNjI0OA.jpg" alt="" width="588" height="349" data-loading-tracked="true" /> 

For the best experience download our chrome extension here: <a href="https://chrome.google.com/webstore/detail/codefresh-for-github-beta/" target="_blank">https://chrome.google.com/webstore/detail/codefresh-for-github-beta/</a>

&nbsp;

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAMxAAAAJDBlYTQ1MDA5LTk2ODktNGI0YS1hNzQ4LTMwODQ2ZjNmNzk1Mg.jpg" alt="" width="588" height="435" data-loading-tracked="true" /> 

Browse to a Github repo and click the Launch button.

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAMMAAAAJDFkNGJiYzdhLTI4MTktNDJjMy04ODMxLWRlYTNmNmFkNjFjYg.jpg" alt="" width="588" height="387" data-loading-tracked="true" /> You can also launch an environment for a specific branch straight from Github:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAANlAAAAJGYyZjU1NWUxLTgwYjctNDU4Ny04N2I0LWYxMDE0NGU1ODI1ZQ.jpg" alt="" width="588" height="277" data-loading-tracked="true" /> Or from a pull request:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/AAEAAQAAAAAAAAKaAAAAJGVmMDFiYzQ0LTIxNjAtNDJiYy1iNTM5LWFlNzc5MGQyYmVlYw.jpg" alt="" width="588" height="238" data-loading-tracked="true" /> 

Obviously you can do all this from the CodeFresh interface as well but with the extension you can actually run any SHA from any public repository on Github with a click.

As you can imagine, CodeFresh makes it incredibly easy to take a new repo and see if it suits your needs, or to test code in a new pull request before merging.

While you don&#8217;t need to know anything about Docker to use CodeFresh, we are Docker based so if you have a Dockerfile in your repo, we can use it to build your environment.

Anyway- that was a quick tour of the app but there are tons more features in there and we&#8217;ll be adding more and more features constantly so take a look and let us know what you think.

<img class="center alignright" src="https://media.licdn.com/mpr/mpr/shrinknp_100_100/AAEAAQAAAAAAAAIRAAAAJDA3N2JhZDUzLWVjYjAtNGFhMC05MjAwLWZkZDU4MTdmZTAxYw.jpg" alt="" width="68" height="67" data-loading-tracked="true" /> If you have any issues, the team will be happy to help- just click on the &#8220;chat&#8221; icon at the bottom right of the site:

Obviously, you can reach out to me as well.