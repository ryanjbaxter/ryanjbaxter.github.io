---
id: 997
title: Bluemix Is Evolving…In A Good Way
date: 2015-02-23T17:53:46+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=997
permalink: /2015/02/23/bluemix-is-evolvingin-a-good-way/
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
---
<div>
  A year ago when Bluemix was first released in beta it was marketed as a word class enterprise platform-as-a-service in the public cloud.  Over the past year Bluemix has evolved in many ways, but two of them have started Bluemix down the path of being more than just a public PaaS.
</div>

&nbsp;

<div>
  First, we introduced <a href="https://console.ng.bluemix.net/solutions/dedicated" target="_blank">Bluemix dedicated</a>.  Bluemix dedicated was a private single tenant instance of Bluemix running in our Softlayer data centers.  This helped address some concerns from customers about having their apps running next to someone else&#8217;s in the cloud.  Bluemix dedicated also had the added benefit of using a VPN connection to connect to services on premise helping enable our customers hybrid cloud scenarios.
</div>

&nbsp;

<div>
  A few months after Bluemix dedicated was announced we introduced the IBM Container service allowing developers to deploy Docker containers to Bluemix.  This was a big step forward as it helped address the concern customers had about having to change their applications to run in a Cloud Foundry based PaaS like Bluemix.  Docker allowed you to take any application, package it in a Docker image, and deploy it to Bluemix without making any changes to your application at all.
</div>

&nbsp;

<div>
  Today at IBM InterConnect IBM announced the next evolution of Bluemix.
</div>

&nbsp;

<div>
  The first major announcement was a revamped IBM Container service.  In its previous iteration, the IBM Container service existed alongside the other services in the Bluemix service catalog.  This always felt “wrong” as it was not a service used by other applications, instead it was a service used to deploy applications just like Cloud Foundry.  For this reason the IBM Container service has been removed from the service catalog in Bluemix and now exists alongside your Cloud Foundry apps deployed in Bluemix.  You can now deploy containers just like you would any other app in Bluemix.
</div>

&nbsp;

<div>
  The second major announcement was that you can now create VMs in Bluemix.  Thats right, a VM just like you would get from an infrastructure-as-a-service provider like SoftLayer.  These VMs live right alongside your Cloud Foundry apps and Docker containers in Bluemix.
</div>

&nbsp;

<div>
  With these changes, Bluemix is no longer a PaaS, it is a true cloud platform.  It is the single place you can go to satisfy all your application deployment needs, whether you want to build a Cloud Foundry application, deploy your app in a Docker container, or need the additional control provided by a VM.
</div>

&nbsp;

<div>
  <a href="http://ryanjbaxter.com/wp-content/uploads/2015/02/Screen-Shot-2015-02-23-at-8.29.58-AM.png"><img class="alignnone size-full wp-image-999" src="http://ryanjbaxter.com/wp-content/uploads/2015/02/Screen-Shot-2015-02-23-at-8.29.58-AM.png" alt="Screen Shot 2015-02-23 at 8.29.58 AM" width="2626" height="1130" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/02/Screen-Shot-2015-02-23-at-8.29.58-AM-300x129.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/02/Screen-Shot-2015-02-23-at-8.29.58-AM-1024x441.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/02/Screen-Shot-2015-02-23-at-8.29.58-AM.png 2626w" sizes="(max-width: 2626px) 100vw, 2626px" /></a>
</div>

&nbsp;

<div>
  The other major announcement was that you can now have Bluemix running locally in your own data center on premise.  With Bluemix public, and Bluemix dedicated and the addition of Bluemix local you can now have Bluemix running where your business needs.  We envision our customers will actually use a combination of Bluemix dedicated and Bluemix local as well as Bluemix public.  For example you may develop and app and deploy it to Bluemix dedicated or Bluemix local and then want to consume the service provided by that app in an app running in Bluemix public.  With these three options a true hybrid cloud is possible using Bluemix.
</div>

&nbsp;

<div>
  <a href="https://console.ng.bluemix.net/solutions/hybrid-cloud"><img class="alignnone size-full wp-image-1000" src="http://ryanjbaxter.com/wp-content/uploads/2015/02/Screen-Shot-2015-02-23-at-8.42.45-AM.png" alt="Screen Shot 2015-02-23 at 8.42.45 AM" width="1740" height="1076" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/02/Screen-Shot-2015-02-23-at-8.42.45-AM-300x186.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/02/Screen-Shot-2015-02-23-at-8.42.45-AM-1024x633.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/02/Screen-Shot-2015-02-23-at-8.42.45-AM.png 1740w" sizes="(max-width: 1740px) 100vw, 1740px" /></a>
</div>

&nbsp;

<div>
  I truly believed with these new announcements that Bluemix has been taken to the next level, it is truly a cloud platform that can satisfy our customers demands.  This coming year should be exciting, there is much more to come!
</div>