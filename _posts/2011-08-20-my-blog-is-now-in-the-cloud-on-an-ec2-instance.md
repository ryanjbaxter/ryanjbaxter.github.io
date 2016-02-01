---
id: 265
title: 'My Blog Is Now In The Cloud&#8230;On An EC2 Instance'
date: 2011-08-20T20:44:07+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/wordpress/?p=265
permalink: /2011/08/20/my-blog-is-now-in-the-cloud-on-an-ec2-instance/
categories:
  - Web Development
---
Up until last week my blog was hosted on wordpress.com.  When I started blogging, I wasn&#8217;t sure how much I was going to actually like it so I didn&#8217;t want to put to much time and money into creating one.  Wordpress.com seemed like the simplest place to set one up.  However I ended up really like blogging and I wanted to do different things with my blog which I couldn&#8217;t do on WordPress.com.  I decided to look into how I could host my own WordPress blog.  I eventually found that there were EC2 images on Amazon&#8217;s cloud that were already setup with WordPress.  Plus if you use a basic EC2 instance it&#8217;s free for a year, you can&#8217;t beat that.

I used one of the EC2 images <a href="http://bitnami.org/stack/wordpress" target="_blank">BitNami has created for WordPress</a>.  Once you have your Amazon web services account its pretty simple to get an image setup.  All you do it click on the one you want on the BitNami site and once its up an running your ready to go.  You can immediately start using the blog.  There are few extra steps you need to do if you want to use a DNS name for your image.  Below are a few links I found useful when setting this up.

&nbsp;

  * <a href="http://paulstamatiou.com/how-to-getting-started-with-amazon-ec2" target="_blank">Getting Started Guide For Amazon EC2</a>
  * <a href="http://wiki.bitnami.org/Applications/BitNami_Wordpress_Stack#How_to_change_the_default_URL.3f" target="_blank">Changing The URL Of Your WordPress Blog</a>
  * <a href="http://wiki.bitnami.org/Native_Installers_Quick_Start_Guide#How_can_I_start_or_stop_the_servers.3f" target="_blank">Starting And Stopping Servers</a>