---
id: 158
title: Containers, Containers Everywhere
date: 2011-01-04T21:14:21+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.wordpress.com/?p=158
permalink: /2011/01/04/containers-containers-everywhere/
jabber_published:
  - 1294175662
  - 1294175662
email_notification:
  - 1294175664
  - 1294175664
categories:
  - OpenSocial
  - Shindig
tags:
  - containers
  - OpenSocial
  - Shindig
---
If you have at all looked into OpenSocial and Apache Shindig than you surley have come across the samples and containers.  The good news is that there are alot, the bad news, well there is little documentation that goes along with the samples and containers.  Without documentation it&#8217;s hard to know what exactly the samples are doing.  Besides that problem there are, or appears to be, three different sample containers that come bundled with Shindig.  In theory all of these containers should be able to render the gadgets in the exact same way, the problem is they all do things in a different ways.

[<img class="alignnone size-full wp-image-159" title="containers" src="http://ec2-107-20-24-186.compute-1.amazonaws.com/wordpress/wp-content/uploads/2011/01/containers.png" alt="" width="226" height="217" />](http://ec2-107-20-24-186.compute-1.amazonaws.com/wordpress/wp-content/uploads/2011/01/containers.png)

&nbsp;

If you are trying to build your own container, you have no idea which sample container is the one you should be modeling your container after.  The container directory actually contains several containers.  These are very trivial and would not work well for a production implementation.  The samplecontainer directory actually contains two containers.  One is rendered when you go to samplecontainer/samplecontainer.html and the other when you go to samplecontainer/examples/commoncontainer/index.html.  Samplecontainer.html is outdated, and you should be modeling your container after commoncontainer/index.html.  This container uses the &#8220;new&#8221; common container code contributed to the project by Google.  I am going to push for the devs of the common container to document a lot of the information about the common container somewhere so it makes it easier for everyone to build their own.