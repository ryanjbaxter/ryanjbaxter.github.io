---
id: 30
title: Sharing Social Media
date: 2010-10-15T00:47:52+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.wordpress.com/?p=30
permalink: /2010/10/15/sharing-social-media/
jabber_published:
  - 1287103673
  - 1287103673
categories:
  - Uncategorized
---
Lately I have been investigating different ways we share social media.  One of the major use cases of social networking sites today is to allow users to share content with others.  How useful would Twitter really be if  people just posted what they were doing, rather than a cool link they just found, or a funny video they just watched?  Most people post links to other content rather than some type of status of what they are doing.  The same is true of Facebook.  Up until a few months ago when you wanted to share some content on a social networking site you usually just pasted a link into your status.  This forced anyone who was interested in the link to leave the site to view the content.  Facebook and Twitter have started to change that.  Now on Facebook if you want to share a YouTube video, and you pasted a link into your status, it will automatically recognizes that the URL points to a YouTube video and will embedd a YouTube player into your status.  #NewTwitter now allows you to expand the tweet to watch the video if the tweet includes a link to a YouTube video.  By doing this Twitter and Facebook allow the user to stay at the site and not leave.

<p style="text-align:center;">
  <a href="http://ec2-107-20-24-186.compute-1.amazonaws.com/wordpress/wp-content/uploads/2010/10/screen-shot-2010-10-14-at-11-38-00-am.png"><img class="size-medium wp-image-32 aligncenter" title="YouTube Embedded In Twitter" src="http://ec2-107-20-24-186.compute-1.amazonaws.com/wordpress/wp-content/uploads/2010/10/screen-shot-2010-10-14-at-11-38-00-am.png?w=241" alt="" width="241" height="300" srcset="http://ryanjbaxter.com/wp-content/uploads/2010/10/screen-shot-2010-10-14-at-11-38-00-am-241x300.png 241w, http://ryanjbaxter.com/wp-content/uploads/2010/10/screen-shot-2010-10-14-at-11-38-00-am.png 483w" sizes="(max-width: 241px) 100vw, 241px" /></a>
</p>

<p style="text-align:left;">
  Gmail is starting to do the same thing with something called <a href="http://code.google.com/apis/gmail/gadgets/contextual/" target="_blank">contextual gadgets</a>.  Have you noticed lately that if you receive an email with a link to YouTube it embeds the player right at the bottom of the email?
</p>

<p style="text-align:center;">
  <a href="http://ec2-107-20-24-186.compute-1.amazonaws.com/wordpress/wp-content/uploads/2010/10/screen-shot-2010-10-14-at-1-07-40-pm.png"><img class="size-full wp-image-37 aligncenter" title="Embedded YouTube Video In Gmail" src="http://ec2-107-20-24-186.compute-1.amazonaws.com/wordpress/wp-content/uploads/2010/10/screen-shot-2010-10-14-at-1-07-40-pm.png" alt="" width="279" height="162" /></a>
</p>

&nbsp;

This is a great start to making sharing of media more useful and productive, but what if you want to share more complex things?  This comes up a lot in the enterprise.  Take for example Lotus Connections.  If you have ever used Connections before, you are familiar with the task of sharing a file with someone. When you do this though, the person you share the file with just gets an email which essentially has a link to the file on Connections.  Clicking on the link forces them to leave their inbox and go to Connections and log in.  Once there though, they can download the file, add a comment, upload another version, or share the file with someone else.  Well what if we could embed the file and all its actions into the email so the user did not have to leave their inbox?  When I say &#8220;embed&#8221; I don&#8217;t mean like an attachment, rather the body of the email will actually point back to Connections to take care of the rendering of the file notification.  This is similar to how embedding YouTube videos work, your not embedding the actual video, instead there is HTML that actually points back to YouTube which renders its own video player.

There is a spec called <a href="http://www.oembed.com" target="_blank">oEmbed</a> which actually specifies how to represent embeddedable media.  Actually YouTube, Flickr, and Twitter, all use or leverage this spec to allow for shared content.  Currently the spec is limited to things that can be embedded by linking to them, like a picture, or by embedding them with simple HTML, like a video.  However files and other types of media cannot be embedded using a URL or HTML.  Furthermore being able to take actions on the embedded media, like adding a comment, is also not possible.

Stay tuned for future posts on thoughts on how we can expand on oEmbed to allow embedding of all kinds of social media.