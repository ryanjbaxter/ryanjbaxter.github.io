---
id: 1042
title: An Introduction To The New IBM Containers Service
date: 2015-07-06T13:25:46+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1042
permalink: /2015/07/06/an-introduction-to-the-new-ibm-containers-service/
dsq_thread_id:
  - 4535001992
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
---
If you logged into Bluemix late last week you might have noticed some changes.  Probably the biggest change you might have noticed is that the IBM Containers service has moved out of beta and is now generally available.  If you are not familiar with the IBM Container service in Bluemix let me spend a minute describing what it does.  The Containers service gives you the option of deploying your application as a Docker container to Bluemix.  If you are not familiar with Docker you can read more about it on the <a href="https://www.docker.com" target="_blank">Docker website</a>.  Docker has become increasingly popular as a way of distributing and running applications because of the portability it provides.  It is similar to VMs but much more light weight and flexible, making it easy to package your application in a Docker container and have it run exactly the same in any environment.

[<img class=" size-full wp-image-1043 aligncenter" src="http://ryanjbaxter.com/wp-content/uploads/2015/07/bluemix_docker.png" alt="bluemix_docker" width="1100" height="550" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/07/bluemix_docker-300x150.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/07/bluemix_docker-1024x512.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/07/bluemix_docker.png 1100w" sizes="(max-width: 1100px) 100vw, 1100px" />](http://ryanjbaxter.com/wp-content/uploads/2015/07/bluemix_docker.png)

&nbsp;

So why choose to run your application as a Docker container vs a Cloud Foundry application in Bluemix?  Good question  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> Probably the #1 reason you might choose Docker over Cloud Foundry is that it allows you to take an existing application and run it in the cloud without any changes to the application code itself.  For example with a Cloud Foundry application you may have to change the code to access some type of cloud service.  With Docker it would be possible to package up the entire application you have today (including the services it uses) and deploy it to the containers service unchanged.  Now I don&#8217;t recommend this approach, you typically want to break out the application and services to separate containers, but it is doable.  Another reason may be that you want to run the application in multiple clouds and Docker can be the common denominator.  As long as you have a Docker host, your application can run on premise, in Bluemix, or in another vendor&#8217;s cloud without worrying about having to make changes for each environment.

&nbsp;

With that said I have put together a video demonstrating the basic functionality of the IBM Containers service in Bluemix.  This should be enough to get you familiar with the service and get you on your way to deploying your own Docker containers to Bluemix.

<iframe width="420" height="315" src="https://www.youtube.com/embed/WMUiBE_7MoU" frameborder="0" allowfullscreen></iframe>

&nbsp;