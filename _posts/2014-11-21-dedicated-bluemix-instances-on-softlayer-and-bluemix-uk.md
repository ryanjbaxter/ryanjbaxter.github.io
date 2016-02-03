---
id: 900
title: Dedicated Bluemix Instances On Softlayer And Bluemix UK
date: 2014-11-21T08:05:14+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=900
permalink: /2014/11/21/dedicated-bluemix-instances-on-softlayer-and-bluemix-uk/
categories:
  - BlueMix
  - Cloud
  - PaaS
---
Greetings from Strata + Hadoop World in Barcelona Spain.  Yesterday was a big day for Bluemix with two major announcements.

The first announcement was the addition of a new <a href="https://developer.ibm.com/bluemix/2014/11/20/bluemix-london/" target="_blank">Bluemix &#8220;region&#8221; in the UK</a>.  At a high level, this means that you can now deploy and run your apps within our UK data center.  This will obviously helps with latency concerns for users you may have in the UK region.  Deploying apps to the UK data center in the Bluemix UI is as simple as selecting the the UK region once logged into Bluemix and following the same process you did before.

[<img class="alignnone size-full wp-image-901" src="http://ryanjbaxter.com/wp-content/uploads/2014/11/Screen-Shot-2014-11-21-at-8.39.51-AM.png" alt="Screen Shot 2014-11-21 at 8.39.51 AM" width="1402" height="424" srcset="http://ryanjbaxter.com/wp-content/uploads/2014/11/Screen-Shot-2014-11-21-at-8.39.51-AM-1024x309.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2014/11/Screen-Shot-2014-11-21-at-8.39.51-AM.png 1402w" sizes="(max-width: 1402px) 100vw, 1402px" />](http://ryanjbaxter.com/wp-content/uploads/2014/11/Screen-Shot-2014-11-21-at-8.39.51-AM.png)

&nbsp;

If you are using the Cloud Foundry command line, the Eclipse plugin, or something like the Maven or Gradle plugins, there is a new API endpoint you need to point those tools at, https://api.eu-gb.bluemix.net.  For example, to point the Cloud Foundry command line at the UK region execute the following command

<pre class="pre">$ cf api https://api.eu-gb.bluemix.net</pre>

The second announcement we made yesterday was around dedicated instances of Bluemix on Softlayer, or for short <a href="https://ace.ng.bluemix.net/?cm_mmc=developerWorks-_-dWdevcenter-_-bluemix-_-lp#/solutions/solution=bluemix_dedicated" target="_blank">Bluemix Dedicated</a>.  With Bluemix Dedicated you get a private, single tenant instance of Bluemix running in ANY Softlayer datacenter that is completely managed by IBM.  With this model you still get all the advantages of a PaaS but in an instance of Bluemix only you have access to.  This solution obviously addresses some of the security concerns certain industries have around running their applications in a multi-tenant environment.  In addition the fact that you can deploy it to any Softlayer datacenter allows you to also address concerns about data leaving the country of origin.  From a functionality perspective Bluemix Dedicated will behave just like the public Bluemix instance and the catalog will contain all the same services you are familiar with.  The one difference is that certain services in the catalog will be replaced with dedicated services.  These dedicated services are private, single tenant versions of their counterparts in the public Bluemix catalog.  There are currently 5 dedicated services, Cloudant, Data Cache, MQ Light, Session Cache, and SQL DB.  Of course you are still free to connect to any public cloud service you want to use and you will be able to connect back to any on premise services you have as well.  Unlike in the public Bluemix instance connections behind your firewall are not done via the Cloud Integration service, instead they are done via a VPN connection or using Softlayer Direct Link.  For more information check out the video below as well as this <a href="https://developer.ibm.com/bluemix/2014/11/20/bluemix-dedicated-cloud-platform/" target="_blank">blog post</a>.

&nbsp;

&nbsp;

&nbsp;

<iframe width="560" height="315" src="https://www.youtube.com/embed/3Xp9hYIUNr8" frameborder="0" allowfullscreen></iframe>

&nbsp;

&nbsp;

&nbsp;

&nbsp;