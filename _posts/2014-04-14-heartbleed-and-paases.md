---
id: 709
title: Heartbleed And PaaSes
date: 2014-04-14T14:30:27+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=709
permalink: /2014/04/14/heartbleed-and-paases/
categories:
  - Cloud
  - PaaS
---
I am sure everyone has heard about the <a href="http://heartbleed.com/" target="_blank">Heartbleed</a> bug by now.  If you haven&#8217;t, where have you been?  Even the <a href="http://www.today.com/tech/protect-yourself-heartbleed-computer-bug-2D79511716" target="_blank">Today show</a> was talking about it <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

> The Heartbleed Bug is a serious vulnerability in the popular OpenSSL cryptographic software library. This weakness allows stealing the information protected, under normal conditions, by the SSL/TLS encryption used to secure the Internet. SSL/TLS provides communication security and privacy over the Internet for applications such as web, email, instant messaging (IM) and some virtual private networks (VPNs).

The bug is pretty nasty if you are using OpenSSL to secure your site, and there is large portion of the internet that is, including this blog.  My blog was created from a prebuilt Bitnami WordPress EC2 image.  I chose the image because it was quick and easy to get up and running.  The downside is that I need to maintain it.  (Yes I could use wordpress.com but I liked the flexibility of hosting it myself.)  This is the second time in a couple years I have had to deal with a security vulnerability.  While coming up with the right steps to fix the vulnerabilities just involved a little bit of Googling and 4 hours total of my life, I am always concerned about whether I am taking the right steps to fix everything.  I am not an admin or an IT guy.  Yes I can fumble my way around and figure out most things, but it is not my speciality.

This got me thinking, if my blog was deployed to a PaaS I wouldn&#8217;t have to worry about this.  This is one of the selling points to using a PaaS solution like BlueMix.  Everything below your application code is taken care of for you, this includes the hardware, networking, OS, and yes the libraries used by the OS (like OpenSSL).  While the Heartbleed bug would have still been a concern for you because of the nature of the security threat, it would not be your responsibility to fix it, it is IBMs and IBM has a lot of people who know exactly how to fix it correctly  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> You as the application developer may need to inform your users that their sensitive information may have been compromised, but that is something you can easily handle.

Will I move my blog to a PaaS solution like BlueMix?  I have thought about it and people have <a href="https://www.ibmdw.net/bluemix/2014/02/17/deploy-wordpress-application-ibm-bluemix/" target="_blank">deployed WordPress to BlueMix</a>.  I just need to look into it a little more and set aside some time <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

P.S. BlueMix was NOT effected by the Heartbleed bug as it is not using OpenSSL.