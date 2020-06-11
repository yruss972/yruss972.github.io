---
id: 137
title: Import Old Blogger Content into WordPress
date: 2009-11-09T13:03:33-07:00
author: Yonah Russ
excerpt: "Recently I've been migrating my old Blogger content to Wordpress. I've been considering this ever since Blogger announced it's new layout / format which required hosting the blog on their servers. Finally I found the energy to tackle the beast but quickly hit my first obstacle- how to import the old Blogger posts."
layout: revision
guid: http://yonahruss.com/wordpress/2009/11/136-revision.html
permalink: /tips/136-revision.html
---
Recently I&#8217;ve been migrating my old Blogger content to WordPress. I&#8217;ve been considering this ever since Blogger announced it&#8217;s new layout / format which required hosting the blog on their servers. Finally I found the energy to tackle the beast but quickly hit my first obstacle- how to migrate the posts.

> If you haven&#8217;t already, you must be using New Blogger and a Google Account on Blogger. If you are still using Old Blogger, the importer will not work. &#8211; <a rel="nofollow" href="http://codex.wordpress.org/Importing_Content">http://codex.wordpress.org/Importing_Content</a>

Importing from my old Atom or RSS feeds didn&#8217;t work.

Importing from a Blogger export file didn&#8217;t work either.

Finally I found a solution which worked for the most part.

  1. I opened a new blog in Blogger and marked it private in the blog settings.
  2. I took the export file from my old blog and imported it into the new blog, converting all the posts to the new Blogger format.
  3. I pointed the WordPress import tool at the new blog and imported all the content- Voila

I can&#8217;t say it worked perfectly:

  * The import tool failed to import all the posts so I had to run it several times. Unfortunately this seemed to duplicate all the comments. Thankfully it didn&#8217;t duplicate the posts. I cleaned up the comments using the WP Ajax Edit Comments plugin.
  * Another issue was that all the tags I had in Blogger turned into WordPress categories. I converted them back to tags using the Categories and Tags Converter- one of the standard Import Tools.

I still have some issues, for example the lack of excerpts for the posts but all in all I&#8217;m very happy with how easy the switch has been so far.