---
id: 343
title: Your Graphic Designer is Tanking Your Site!
date: 2015-01-22T09:27:05-07:00
author: Yonah Russ
layout: post
guid: http://www.yonahruss.com/?p=343
permalink: /performance/your-graphic-designer-is-tanking-your-site.html
categories:
  - Performance
tags:
  - Cascading Style Sheets
  - Google PageSpeed Tools
  - graphic designer
  - Graphics file formats
  - Image compression
  - JPEG
  - Lossy compression
  - Portable Network Graphics
  - Progressive JPEG
  - Technology_Internet
  - Transparency
  - Web design
---
<img class="aligncenter size-full wp-image-344" src="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/mona.jpg" alt="mona lisa" width="698" height="400" srcset="http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/mona.jpg 698w, http://www.yonahruss.com/wordpress/wp-content/uploads/2016/01/mona-300x172.jpg 300w" sizes="(max-width: 698px) 100vw, 698px" />

Your graphic designer is an artist, a trained expert in aesthetics, a master at conveying messages via images and feelings via fonts. He may also be slowing your site down so much that nobody is seeing it.

Artists tend to be heavy on the quality and lighter on the practicality of what they deliver. It&#8217;s not entirely their fault. Even the most conscientious and experienced designer needs to sell his work and quality sells. Do marketing departments want to see their company advertised in 4K glory or on their mom&#8217;s 19&#8243; LCD?

## Quality isn&#8217;t worth the cost.

The reality of the Internet is that too much quality costs more than it&#8217;s worth. It costs bandwidth and it costs customers who aren&#8217;t willing to wait for heavy sites to load.

Is there a subliminal value to high quality graphics? The answer is yes but only if someone sees them.

## How much do the images make a difference?

Here is a quick experiment you can do to visualize some of the improvement you could see. Go into your browser settings and disable all images. Then go back and visit your website to see how it loads without the bloat.

I just tried this on a the homepage of a major news network. With images the site was over 4MB. You don&#8217;t even see most of the pictures that were loaded. Without images the site was under 2MB (still very high to be honest). That basically means that, according to the laws of physics and in the best case scenario, the site with images will take at least twice as long to load.

You might say they are a huge media site. They know what they&#8217;re doing. They need the images. The sad truth is that they are wasting your bandwidth for no good reason as I&#8217;ll demonstrate shortly.

## How to fix it?

I won&#8217;t tell you to get rid of all the images on your site, though if that can fit with your needs, less is always faster. You do, however, need to optimize your images for the web.

As a performance engineer who has examined the performance of literally hundreds of websites, the number one problem is always images that haven&#8217;t been optimized. Just optimizing your images properly can cut the download time of a website in half, possibly more.

As a continuation of our experiment above I took this picture from their page and tried optimizing it to see what kind of savings I could get. Here is the original 41KB version:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/p/5/005/0b1/051/2dadacf.jpg" alt="" width="588" height="331" data-loading-tracked="true" /> Here is the optimized 15KB version:

<img class="center aligncenter" src="https://media.licdn.com/mpr/mpr/shrinknp_800_800/p/5/005/0b1/051/387f56b.jpg" alt="" width="588" height="331" data-loading-tracked="true" /> 

The result is almost 1/3 the size and I don&#8217;t think you can tell the difference. Would you notice any difference if this was just another image on a website? You too could get a 50-60% performance boost, just by optimizing your images.

If you are using some type of automated deployment for your sites, there are tools which will optimize your images automatically. They are OK for basic optimizations.

To really optimize your images, you need a person with a realistic eye (graphic designers are always biased towards heavier higher quality images) and you need to follow these basic rules:

## Use the right images for the job.

In general, properly compressed, Progressive JPEG files are the best to use but they don&#8217;t support transparency. After weighing carefully if you need a transparent image or not, use Progressive <a href="https://en.wikipedia.org/wiki/JPEG" target="_blank" rel="nofollow">JPEG</a> files for any image that has more colors than you can count on your hand and doesn&#8217;t require transparency. Otherwise use a PNG file.

## Optimize the images.

### Optimizing JPEG files

First, manually reduce the file&#8217;s quality level until you reach the setting with acceptable levels of pixelation in the sharp edges and gradient surfaces of the image.

Note that you should always start optimizing from the original image at it&#8217;s best quality. Repeatedly editing a JPEG file will degrade the quality of the image with each generation.

After you have reached the optimal quality level, run them through a tool like<a href="https://imageoptim.com/" target="_blank" rel="nofollow">ImageOptim</a> which will remove any thumbnails or metadata adding weight to the image.

### Optimizing PNG files

Optimize PNG files first by running them through a tool like <a href="http://pngmini.com/" target="_blank" rel="nofollow">ImageAlpha</a> or<a href="https://tinypng.com/" target="_blank" rel="nofollow">TinyPNG</a>. These tools use lossy compression techniques to reduce the number of colors in your image fitting them into a smaller bitmap, and resulting in better compression than a PNG would normally have.

Note: ImageAlpha gives you more control over the process letting you decide how many colors should be used in the resulting image. This is very useful for transparent PNGs with very few colors.

Then run the images through ImageOptim or similar tool to squeeze some extra space out of them.

Once you have mastered the above, your site should be much lighter. If you want to optimize further (and you should), look into the following techniques:

## Don&#8217;t use images where you don&#8217;t have to.

Many of the most common icons used on websites have been implemented as fonts. Once a font is loaded, each icon is a single character and making the icon larger or smaller is as simple as using a larger font size.

Note: There is some overhead in loading and using the fonts (additional CSS) so use wisely.

## Combine small images.

Similar to the idea of using fonts, is the CSS Sprite. These combine multiple small images into a single transparent PNG file and then show only one image at a time on your site using CSS tricks.

The advantage of this technique is that it saves requests to the web server. Each request to a web server has latency, sometimes as much as 200ms, which can&#8217;t be eliminated and for small images, this request latency can sometimes account for more time than downloading the image itself. By combining the smaller images in your site, you save that overhead.

There are tools which will generate the combined images and the corresponding CSS for you.

## Summary

I&#8217;ve used these image optimization techniques in hundreds of websites resulting in significant savings of bandwidth and increased performance. Most importantly, though, these techniques result in better visitor engagement.

If you&#8217;re interested in optimizing your site for best cost/performance, feel free to contact me via <a href="https://il.linkedin.com/in/yonahruss" target="_blank" rel="nofollow">LinkedIn</a> or via <a href="https://donatemyfee.org/" target="_blank" rel="nofollow">https://donatemyfee.org/</a>.