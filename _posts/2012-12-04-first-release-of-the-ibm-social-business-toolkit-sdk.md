---
id: 383
title: First Release Of The IBM Social Business Toolkit SDK
date: 2012-12-04T12:47:22+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=383
permalink: /2012/12/04/first-release-of-the-ibm-social-business-toolkit-sdk/
categories:
  - Web Development
---
Today was the first release of the IBM Social Business Toolkit SDK.  The SDK is meant to help developers build apps using IBMs Social Business Platform, including products like IBM Connections, IBM (Lotus) Domino, IBM Sametime, IBM SmartCloud For Social Business, and many more.  The initial release of the SDK has APIs which focus on IBM Connections and IBM SmartCloud.  Not all APIs in Connections and SmartCloud have support within the SDK yet but that will come in the future.  The good news is that once you understand how to use the SDK to call an endpoint (ie a REST API) it is VERY simple to call any API, whether it is an API for an IBM product or not.  This initial release has Javascript and Java APIs.  We made this choice because Java and Javascript are the most common languages used by not only developers integrating into IBMs portfolio but in <a href="http://redmonk.com/sogrady/2012/09/12/language-rankings-9-12/" target="_blank">general as well</a>.  Javascript is also used in almost every web app.  It doesn&#8217;t matter if you are a Java, PHP, Ruby, Node.js, or XPages developer all of these languages can use Javacript on the front end.  The plan is that in future releases we will have support for other languages beyond just Java and Javascript.  Also since the project is open source, feel free to contribute API libraries for your favorite language  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> The Javascript portion of the SDK is also meant to be used along side a higher level Javascript library.  Initially the SDK has support for various versions of Dojo , but support for JQuery is planned for in the future.  If you are a developer I encourage you to start taking a look at the SDK by <a href="http://ibmsbt.openntf.org/" target="_blank">downloading the source</a> and getting it up and <a href="http://www-10.lotus.com/ldd/appdevwiki.nsf/xpDocViewer.xsp?lookupName=IBM+Social+Business+Toolkit+SDK+documentation#action=openDocument&res_title=Installing_the_SDK_SDK1.0&content=pdcontent" target="_blank">running on your favorite app server</a>.  Also if you want to get started setting up a J2EE development environment for the SDK you can follow along with this video (written instructions will be available soon).

<span class="youtube"></span>

The central place for all things related to the SDK can be found on this <a href="https://www.ibm.com/developerworks/mydeveloperworks/groups/service/html/communityview?communityUuid=0f357879-ccee-4927-98c1-7bb88d5dc81f" target="_blank">community</a> on developerWorks.  Here are some other videos that provide great overviews of the SDK.

<span class="youtube"></span>

<span class="youtube"></span>