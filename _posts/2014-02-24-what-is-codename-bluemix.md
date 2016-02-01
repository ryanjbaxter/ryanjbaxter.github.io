---
id: 493
title: 'What Is Codename: BlueMix?'
date: 2014-02-24T12:16:45+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=493
permalink: /2014/02/24/what-is-codename-bluemix/
dsq_thread_id:
  - 4535001764
categories:
  - BlueMix
  - Cloud
  - PaaS
---
**BlueMix will help you save Earth from a robot apocalypse&#8230;.**

<span class="youtube"></span>

&nbsp;

Just kidding&#8230;.but it is a cool marketing video for BlueMix <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

&nbsp;

OK so what is BlueMix REALLY&#8230;.

As I said in my previous post <a href="http://ng.bluemix.net" target="_blank">BlueMix</a> is IBM&#8217;s new PaaS (platform as a service) offering.  The open beta was announced today at IBM Pulse.  For those of you that are wondering where PaaS fits in the rest of the cloud stack, here is a pretty good diagram.

&nbsp;

[<img class="alignnone size-full wp-image-494" alt="Cloud Stack" src="http://ryanjbaxter.com/wp-content/uploads/2014/02/figure1-new.jpg" width="750" height="374" />](http://ryanjbaxter.com/wp-content/uploads/2014/02/figure1-new.jpg)

&nbsp;

Basically PaaS fits between IaaS (SoftLayer, Amazon AWS, etc) and SaaS (SmartCloud For Social Business).  You may be familiar with other PaaS offerings such as <a href="https://www.heroku.com/" target="_blank">Heroku</a>, <a href="https://www.windowsazure.com/en-us/" target="_blank">Windows Azure</a>, <a href="http://www.gopivotal.com/paas" target="_blank">Pivotal One</a>, and <a href="https://www.openshift.com/" target="_blank">OpenShift</a>.  The best part of PaaS, from a developer perspective, is that it is an abstraction layer between you and the hardware.  When you use a PaaS you have no idea about what hardware or OS you are running on, nor do you need to care.  For developers this is the holy grail, no developer wants to have to setup servers, manage OSes, deploy DBs, etc, you just want to write code and deploy your app.  Every time you need to stop and configure/deploy something you are not writing code.  Plus lets face it, developers, including me, are not very good administrators.

Most PaaSes also provide various services that apps can use.  Need a DB for your app?  No problem just bind it to the app and BAM! you have a DB you can use.  Need to push notifications to a mobile device?  No problem there is a service you can use.  Need to send email notifications?  Sure we can do that too.  Need to do some analytics?  You got it!  All of these services and more are available to developers so they don&#8217;t need to worry about implementing it themselves or deploying a separate app to do it for them (not all of these are available in BlueMix yet, that will change over time).  In addition PaaSes don&#8217;t usually care about the language you want to use (to a certain point, I don&#8217;t think anyone is going to support LotusScript for example).  Java, Node.js, PHP, Python, Ruby, and many more can all be used.  Essentially you can use any language you want in BlueMix as long as there is a <a href="https://github.com/cloudfoundry-community/cf-docs-contrib/wiki/Buildpacks" target="_blank">buildpack</a> for it.  And if there isn&#8217;t a buildpack, you can make your own!  So what does this all amount to for you as a developer?  A HUGE TIME SAVINGS!  All developers have to do is code and deploy.

[<img class="alignright size-full wp-image-512" alt="cf-logo" src="http://ryanjbaxter.com/wp-content/uploads/2014/02/cf-logo.jpeg" width="225" height="225" srcset="http://ryanjbaxter.com/wp-content/uploads/2014/02/cf-logo-150x150.jpeg 150w, http://ryanjbaxter.com/wp-content/uploads/2014/02/cf-logo.jpeg 225w" sizes="(max-width: 225px) 100vw, 225px" />](http://ryanjbaxter.com/wp-content/uploads/2014/02/cf-logo.jpeg)

BlueMix is also based on an open source project called <a href="http://www.cloudfoundry.com/" target="_blank">Cloud Foundry</a>.  Cloud Foundry was originally started by Pivitol Labs, and quickly picked up a number of contributors.  In July of last year IBM and Pivitol <a href="http://www-03.ibm.com/press/us/en/pressrelease/41569.wss" target="_blank">announced</a> they would be working together to enhanced the Cloud Foundry project.  Since BlueMix is based on an open source PaaS implementation it makes it easier to avoid vendor lockin with the PaaS you built your app on.

You can learn more about BlueMix and sign up for the open beta <a href="http://www-01.ibm.com/software/ebusiness/jstart/robot/?ce=ISM0007&ct=swg&cmp=ibmsocial&cm=h&cr=mf&ccy=us" target="_blank">here</a>.  It has everything you need to know.  Also be sure to check out the videos below.

<span class="youtube"></span>

Here is a video from my colleague David Barnes.  (slightly outdated, but still gets the point across.)

<span class="youtube"></span>

&nbsp;