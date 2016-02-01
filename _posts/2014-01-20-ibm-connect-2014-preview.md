---
id: 473
title: IBM Connect 2014 Preview
date: 2014-01-20T09:56:03+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=473
permalink: /2014/01/20/ibm-connect-2014-preview/
categories:
  - Connect
  - IBM Social Business Toolkit
---
IBM Connect 2014 is just a few days away, I can&#8217;t believe it!  I will be speaking again this year and again doing a revamp of the JumpStart on OpenSocial I did last year.  The JumpStart (JMP103) will be on Sunday at 8AM, so please don&#8217;t party too hard on Saturday night and come and check it out.  I decided it would be good to give everyone a quick preview of what we will be talking about and demoing this year, so I have created a quick (18 min) video.

<span class="youtube"></span>

As you can see I have been hard at work over the past few months integrating support for rendering OpenSocial gadgets into the IBM Social Business Toolkit Playground.  One of the main difficulties with building OpenSocial gadgets has been learning about the different features and then setting up an environment to test them.  That is why shortly after Connect 2013 I began work on a project called the <a href="http://opensocial.github.io/explorer/" target="_blank">OpenSocial Explorer</a>.  This project was open sourced as part of the OpenSocial Foundation and is intended to be used by any gadget developer.  It is a J2EE based app, that uses Dojo and Bootstrap on the front end.  This project is a great start but from an IBM perspective it has a few issues.  First, with the Playground we already had a tool that developers were using to learn more about the IBM Social Business APIs, so why force developers to use a second one?  Second, since the OpenSocial Explorer is a generic tool for any platform based on OpenSocial it didn&#8217;t show developers how to build gadgets for the IBM Social Business Platform.  To solve these problems I integrated all the features and functionality from the OpenSocial Explorer into the Playground, so now you can do everything you can do in the OpenSocial Explorer in the Playground&#8230;and more!

Over the coming days and weeks we will be doing a release of the IBM SBT SDK on <a href="http://ibmsbt.openntf.org" target="_blank">OpenNTF</a> which will include all the code and an updated NSF allowing you to deploy the Playgorund with OpenSocial features to your own Domino server.  If you want to check it out today you can clone the git repo and do a build to get the binaries.  We are also in the process of getting an updated Playground deployed to <a href="https://greenhouse.lotus.com/sbt/SBTPlayground.nsf/" target="_blank">Greenhouse</a>.  Hopefully this will happen before Connect.

If you are an XPages developer I am sure you are saying &#8220;Hey, I know the Playground is an XPages app, how can I render OpenSocial gadgets in my XPages app?&#8221;.  The answer is you can!  The code I wrote to render gadgets in the Playground is extensible, meaning all you have to do is implement some interfaces in your XPages app (or OSGi bundle) and you can also render OpenSocial gadgets.  I will warn you upfront that <span style="color: #ff0000;"><strong>this code is experimental and the APIs and functionality WILL change over</strong> <strong>time</strong>.<span style="color: #000000;">  <span style="color: #333300;">However I believe it is important to allow people to experiment with the functionality in order to learn more about the different use cases people will come up with.  I am not going to go in depth about how you render OpenSocial gadgets in your own app in this blog post but I will point you to some <a href="https://github.com/OpenNTF/SocialSDK/wiki/Building-Your-Own-OpenSocial-Container" target="_blank"><span style="color: #333300;">documentation</span></a> to learn more.</span></span></span>_
  
_ 

I hope you can all make it my session on Sunday, but if not the slides and recordings will be available sometime after Connects ends.