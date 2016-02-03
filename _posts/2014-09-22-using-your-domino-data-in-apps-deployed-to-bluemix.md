---
id: 874
title: Using Your Domino Data In Apps Deployed To Bluemix
date: 2014-09-22T08:16:16+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=874
permalink: /2014/09/22/using-your-domino-data-in-apps-deployed-to-bluemix/
categories:
  - Uncategorized
---
Over the past few weeks I have done presentations on Bluemix at two ICS user groups, MWLUG and ICONUK.  I did pretty much the same presentation at both mainly focused on introducing developers to Bluemix.  Obviously, since these user groups are focused mainly around ICS, there is interest in how Domino and XPages fits into the Bluemix story.  Well as of right now you cannot deploy your XPages apps to Bluemix.  However, Kramer Reeves announced at MWLUG that ICS is investigating how to make this dream come true.  As we all know an investigation is no guarantee, so what is a Domino developer to do in the mean time?  While you can&#8217;t deploy your XPage applications, there is no reason why you can&#8217;t use the data stored in your NSFs.  You just need to use another runtime, such as Java, Node.js, Ruby, PHP, or any other runtime Bluemix supports, to build the front end and business logic of your application.

So how do you do it?  Well you start by familiarizing yourself with Domino Access Services.  Domino Access Services (DAS) essentially allows you to access and manipulate the data within your NSFs over HTTP using REST.  The good thing about DAS is that you can get this functionality without having to write any code.  You essentially enable DAS on your Domino server and then enable it on the NSFs you want to access over HTTP.  This is all spelled out in the documentation that can be found <a href="http://www-10.lotus.com/ldd/ddwiki.nsf/xpAPIViewer.xsp?lookupName=IBM+Domino+Access+Services+9.0.1#action=openDocument&content=catcontent&ct=api" target="_blank">here</a>.  Once you have DAS enabled any application that has an internet connection can access and manipulate the data within your NSF, this includes mobile apps, web apps, or any app deployed to Bluemix as long as the Domino server is accessible to the public internet.  All you need to do is make HTTP requests.

However there are many situations where your Domino server many not be accessible to the public internet, what do you do then?  Your first instinct might be to try to setup a VPN connection.  Well that is not possible with Bluemix because Bluemix is a PaaS.  You can&#8217;t even SSH into the machine your app is running on in Bluemix never mind setup a VPN connection.  The good news is that there is a service in Bluemix called Cloud Integration that you can use to connect to pretty much any server that might be running on a private network or be behind a firewall.  I wrote a separate blog post on using this service, you can read more about it <a title="Deploying Your Hybrid Cloud Apps Has Never Been Easier With Bluemix" href="http://ryanjbaxter.com/2014/08/28/achieving-the-elusive-hybrid-cloud-with-bluemix/" target="_blank">here</a>.  By using the Cloud Integration service in Bluemix you can then use the REST APIs on your Domino server from your Bluemix application even if those REST APIs are not exposed to the public internet!  Pretty cool stuff.

To demonstrate this I have created a short video showing you how.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

<iframe width="420" height="315" src="https://www.youtube.com/embed/322NPTgUS7E" frameborder="0" allowfullscreen></iframe>

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;

In addition my session from MWLUG was recorded, so you can watch that as well.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

<iframe width="420" height="315" src="https://www.youtube.com/embed/LwnI9ZLJ5tU" frameborder="0" allowfullscreen></iframe>

&nbsp;

&nbsp;

&nbsp;

&nbsp;

Finally you can find my slides from MWLUG on SlideShare.

<iframe src="//www.slideshare.net/slideshow/embed_code/key/GUhfQeojKcLDnb" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/RyanBaxter/mwlug-bluemix-38465731" title="MWLUG Bluemix" target="_blank">MWLUG Bluemix</a> </strong> from <strong><a href="//www.slideshare.net/RyanBaxter" target="_blank">Ryan Baxter</a></strong> </div>