---
id: 1087
title: Using Microservices To Build Cloud Native Applications – Part 2
date: 2015-07-22T14:19:26+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1087
permalink: /2015/07/22/using-microservices-to-build-cloud-native-applications-part-2/
categories:
  - BlueMix
  - Cloud
  - PaaS
---
In [part 1](http://ryanjbaxter.com/2015/07/15/using-microservices-to-build-cloud-native-applications-part-1/) of this 2 part post, I described what microservices are and how I have gone about transitioning a 3-tier monolithic application to use a microservices architecture.  If you have not read [part 1](http://ryanjbaxter.com/2015/07/15/using-microservices-to-build-cloud-native-applications-part-1/), I suggest you head back and read that now.  Also, if you have not read <a href="http://ryanjbaxter.com/2015/07/13/building-cloud-native-applications/" target="_blank">my post on building cloud native applications</a> I suggest you check that out as well to better understand the bigger picture of why microservices are important when building cloud applications.

At the end of part 1 we left off with an application that had been broken apart into several microservices all working together to produce the overall experience for the end user.  The architecture looked like this

[<img class="alignnone size-full wp-image-1073" src="http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices4.jpg" alt="intro-to-microservices" width="1024" height="640" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices4-300x188.jpg 300w, http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices4.jpg 1024w" sizes="(max-width: 1024px) 100vw, 1024px" />](http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices4.jpg)

While this architecture was much more suited for the cloud than our original monolithic architecture, I mentioned that there was still some problems we needed to address.

The first problem has to do with fragility, and as I mentioned in <a href="http://ryanjbaxter.com/2015/07/13/building-cloud-native-applications/" target="_blank">my post about building cloud native applications</a>, fragility is the enemy.  Where is the fragility in this architecture?  Well it is actually between the client side code and the services.  As it stands right now, our client side code has a hard dependency on all the services.  Here is a good example as to why this is bad.  It is very common in microservice applications for the services to evolve.  Typically this involves one microservice growing too large and needing to be split up into two or more new services, but it also might involve a service being completely removed.  When something like this occurs (an it is bound to happen at some point in our application&#8217;s lifetime) our client side code is suddenly broken because it is communicating directly with that evolving service.

A more subtle problem has to do with the type of client communicating with the services.  Take, for example, a desktop and a mobile phone.  The two clients are very different, especially when it comes to network bandwith.  It might be OK for our web app to be making multiple requests to each individual service in order to render the UI when on a desktop browser but on a mobile phone it might be better to make a smaller number of requests and increase the payload response size.

Bottom line is we need something in-between our clients and our services that can provide our clients with an abstraction layer.  For this reason, microservice applications typically have what is called an &#8220;API Gateway&#8221; that clients use.  If we introduce an API Gateway into our sample application here is what it would look like

[<img class="alignnone size-full wp-image-1089" src="http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices6.jpg" alt="intro-to-microservices" width="1024" height="640" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices6-300x188.jpg 300w, http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices6.jpg 1024w" sizes="(max-width: 1024px) 100vw, 1024px" />](http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices6.jpg)

In this diagram the API Gateway also serves up the client side code, but that does not have to be the case.  The important part is that the API Gateway knows how to talk to the services and as services evolve we can avoid making changes to the client side code and just let the API Gateway know about the changes to the services.  The API Gateway can also do aggregation of service calls to optimize requests for different types of devices if we need to.

While using an API Gateway is better than having the clients talk directly to the services we can do a little better.  Here is another problem to consider, what happens when we scale a service in this architecture?

[<img class="alignnone size-full wp-image-1090" src="http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices7.jpg" alt="intro-to-microservices" width="1024" height="640" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices7-300x188.jpg 300w, http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices7.jpg 1024w" sizes="(max-width: 1024px) 100vw, 1024px" />](http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices7.jpg)

In this picture we have scaled the Sessions Service but the API Gateway has no idea about the new service instance so how can it take advantage of the additional instance?  We would have to change the API Gateway so it knows about the second instance.  With features like auto-scaling provided by the cloud platform we are deployed to and the fact we might have 100s or services, modifying the API Gateway would just not scale.

To solve this problem, microservice applications typically use a Service Discovery application which allows all microservices to register themselves and then broadcast their existence to other services in the application.  Here is what that looks like.

[<img class="alignnone size-full wp-image-1091" src="http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices8.jpg" alt="intro-to-microservices" width="1024" height="640" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices8-300x188.jpg 300w, http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices8.jpg 1024w" sizes="(max-width: 1024px) 100vw, 1024px" />](http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices8.jpg)

With our Service Discovery application now deployed, each service instance registers itself with the Service Discovery application and in turn also queries the Service Discovery application for what other services are available to it.  This solves the scaling problem with our API Gateway.  As we bring up more instances of XYZ service they will all register themselves with the Service Discovery application and the API Gateway will periodically query the Service Discovery application to make sure it has an updated list of services.  Once the new instances have been registered with the Service Discovery application the API Gateway will get them and then be able to leverage those new instances when making requests to the service.  All this can be done dynamically without changing and code in the API Gateway, which is exactly what we want.

There are additional benefits to using a Service Discovery application as well.  Consider the case where we have the same microservices deployed across multiple datacenters.  Each datacenter has a deployment that looks like the architecture above.  The Service Discovery applications in each datacenter can share data about the services running in that datacenter with each other.  This would allow the application to be unaffected by a complete outage of a given service in one datacenter given the service is still available in another datacenter. This type of functionality will give our application even more resiliency to failures.

At this point we have a pretty good microservices based architecture to use for our application.  The one thing to note about this architecture is that there are many more moving parts than our original 3-tier architecture had.

[<img class="alignnone size-full wp-image-1068" src="http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices.jpg" alt="intro-to-microservices" width="1024" height="640" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices-300x188.jpg 300w, http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices.jpg 1024w" sizes="(max-width: 1024px) 100vw, 1024px" />](http://ryanjbaxter.com/wp-content/uploads/2015/07/intro-to-microservices.jpg)

The bottom line is that like everything you do there are tradeoffs to the architectures you choose.  If you need resiliency in the cloud, want to gain speed in development, and really be a truly agile organization there is no doubt that microservices give you that.  However, you need to weigh those benefits against the additional challenges introduced by a more complex architecture.  It is clear given the interest amongst members of the cloud community in microservices that there are many organizations facing these challenges head on and looking for solutions.  It also clear that when done right, as proven by Netflix, Amazon, and others, microservices can make your applications and organization better.  The decision of whether you should use microservices in your application is ultimately up to you, but I personally believe that it is a good solution for a certain class of cloud applications.

Now comes the follow up question to this discussion, &#8220;Are there tools out there that can help me build microservices?&#8221;  The answer is a resounding &#8220;Yes&#8221; and I will discuss some of these tools in future posts.