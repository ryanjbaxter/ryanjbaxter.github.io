---
id: 1053
title: Building Cloud Native Applications
date: 2015-07-13T13:10:27+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1053
permalink: /2015/07/13/building-cloud-native-applications/
dsq_thread_id:
  - 4535002007
categories:
  - Cloud
  - PaaS
---
The nature of my job forces me to build quick simple sample applications that are easy to understand and demonstrate a single concept.  After all I am trying to enable developers, and making something overly complex generally turns developers off when they are trying to learn something new.  However I often hear questions about how to build real world production grade applications that run in the cloud.  This is something that is very hard to demonstrate simply because, lets face it, no production quality application has ever been considered simple to understand.  However, I do think it is an important topic to address because if you are serious about building an application in the cloud you should understand how cloud applications are different from your everyday app that everyone is familiar with building.

In my opinion, the cloud was originally marketed to developers as &#8220;the same thing you have today but a vendor is taking care of the infrastructure&#8221;.  In some ways this is true, take any cloud vendor that allows you to request a VM from them.  It feels just like the VM you had running in your own data center for the most part.  The only difference is that its NOT IN YOUR DATA CENTER.  The fact that it is not running in your datacenter is exactly the reason why cloud applications need to be architected differently.  The lack of absolute control and predicability of the cloud is what makes it necessary to think differently when running applications in the cloud.  Everything from the hardware to the networking is all someone elses responsibility, and you can&#8217;t even get that person on the phone if you think something is wrong.  On top of that, if you are serving millions upon millions of people every day (i.e. Netflix, Amazon, Google) by the time your realize something is wrong it is too late, you have already lost a huge chunk of money.

So at the end of the day if you are serious about building and running an application in the cloud you need to first understand that you should not be building a cloud application like it is running in your own data center.  You need to build it with every intention of it running in a very volatile place where much of what lies underneath your application is completely our of your control.  A term has recently emerged for applications that have built to be run in the cloud, we refer to them as &#8220;cloud native applications&#8221;.  So what makes an application a cloud native application?  In Matt Stine&#8217;s book &#8220;_<a href="http://www.oreilly.com/programming/free/migrating-cloud-native-application-architectures.csp" target="_blank">Migrating To Cloud-Native Application Architectures</a>_&#8221; he describes cloud native applications as having the following characteristics

  * Is a <a href="http://12factor.net" target="_blank">12-factor application</a>
  * Follows a microservices architecture
  * Uses self-service agile infrastructure &#8211; AKA a Platform-as-a-service
  * Uses API-based collaboration
  * Is antifragile

[<img class="alignnone size-full wp-image-1059" src="http://ryanjbaxter.com/wp-content/uploads/2015/07/cat.gif" alt="cat" width="180" height="270" />](http://ryanjbaxter.com/wp-content/uploads/2015/07/cat.gif)

&nbsp;

Lets look at each of these in more detail.

&nbsp;

### 12-Factor Applications

[<img class="alignnone size-full wp-image-1060" src="http://ryanjbaxter.com/wp-content/uploads/2015/07/12factor.gif" alt="12factor" width="601" height="174" />](http://ryanjbaxter.com/wp-content/uploads/2015/07/12factor.gif)

When <a href="https://www.heroku.com/" target="_blank">Heroku</a> was first introduced it was such a new concept that the engineers there came up with a set of guidelines for developers trying to use the platform.  There ended up being 12 common practices that all successful applications deployed to Heroku had in common, they called these applications 12-Factor Applications.  All of the applications had the following in common:

  1. Once codebase tracked in revision control, multiple deploys
  2. Explicitly declare and isolate dependencies
  3. Store config in the environment
  4. Treat backing resources as attaches resources
  5. Sticky separate build and run stages
  6. Execute the app as one or more stateless resources
  7. Export services via port binding
  8. Scale out via the process model
  9. Maximize robustness with fast startup and graceful shutdown
 10. Keep development, staging, and production as similar as possible
 11. Treat logs as events streams
 12. Run admin/management tasks as one-off processes

I won&#8217;t describe the details behind each one of these guidelines, instead I will let you read more about them on <a href="http://12factor.net" target="_blank">12factor.net</a>.

&nbsp;

### Microservices Architecture

[<img class="alignnone size-full wp-image-1061" src="http://ryanjbaxter.com/wp-content/uploads/2015/07/micro-service-architecture.png" alt="micro-service-architecture" width="600" height="282" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/07/micro-service-architecture-300x141.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/07/micro-service-architecture.png 600w" sizes="(max-width: 600px) 100vw, 600px" />](http://ryanjbaxter.com/wp-content/uploads/2015/07/micro-service-architecture.png)

If you have not heard of the term microservices yet than you have not been looking into building cloud applications for too long  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> You cannot read a blog post or attend a conference on cloud today without hearing something about it.  What is it?  I think <a href="http://martinfowler.com/articles/microservices.html" target="_blank">Martin Fowler and James Lewis</a> describe it best:

> In short, the microservice architectural style is an approach to developing a single application as a suite of small services, each running in its own process and communicating with lightweight mechanisms, often an HTTP resource API.

In essence we are all used to building large monolithic applications.  These applications typically package everything into a single artifact that needs to be built, tested, and deployed to an application server and typically has a <a href="https://en.wikipedia.org/wiki/Multitier_architecture#Three-tier_architecture" target="_blank">3 tier architecture</a>.  So why do monolithic apps not work in the cloud?  One reason is they are slow.  Building them is slow, testing them is slow, deploying them is slow.  With cloud native applications we want as much speed as possible.  They are also very fragile, if one component in the monolith crashes everything goes down with it.  This is not acceptable in the cloud.  They are also not easy to scale.  If I need to scale a single component of my application I end up scaling the entire monolith.  This requires me to consume more infrastructure and therefore pay more money to my cloud provider.

When you build an application using microservices all of these issues go away and your application is much more suited for the cloud.  Much of what it means to build a cloud native application builds off of using microservices including the rest of the characteristics mentioned in this blog post.  I will look at microservices in detail in future blog posts ([Part 1](http://ryanjbaxter.com/2015/07/15/using-microservices-to-build-cloud-native-applications-part-1/)).

&nbsp;

### Self-Service Agile Infrastructure

Another term for Self-Service Agile Infrastructure is platform-as-a-service.  In essence when deploying cloud native applications you do not want to go through the process of requesting a VM, configuring that VM, securing that VM, installing dependencies, deploying your application, configuring your application, and setting up tools to monitor your application, you want my platform to do that for you.  Remember, if our cloud native application is using microservices it can potentially have 100s of services that need to be deployed in order for the application to work.  Going through the above process for each microservice is not going to scale, no matter how many people work on the team.

Contrast the above process to one where you use a platform-as-a-service.  All you need to do is hand the platform your application code and it takes care of the rest.  Of course this can also be automated via tools like Jenkins and other build and deploy tools (perhaps your platform has these tools built in, i.e. IBM Bluemix and DevOps Services).

The self-service agile infrastructure should also be responsible for providing your application with the backing services is needs to run.  Things like databases, message queues, and caching services should be able to requested on demand and provisioned rapidly when an application is deployed.

&nbsp;

### API Based Collaboration

[<img class="alignnone size-full wp-image-1062" src="http://ryanjbaxter.com/wp-content/uploads/2015/07/Apigee-Heart-APIs-sticker.jpg" alt="Apigee Heart APIs sticker" width="400" height="307" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/07/Apigee-Heart-APIs-sticker-300x230.jpg 300w, http://ryanjbaxter.com/wp-content/uploads/2015/07/Apigee-Heart-APIs-sticker.jpg 400w" sizes="(max-width: 400px) 100vw, 400px" />](http://ryanjbaxter.com/wp-content/uploads/2015/07/Apigee-Heart-APIs-sticker.jpg)

When you are following the microservice design pattern you are going to need a new way for the components in your application to communicate.  You cannot rely on just making method calls between the components of your application anymore.  Having versioned REST APIs is the most common way for your services to communicate in cloud native applications.  These may not be public APIs you let external consumers use, but you should consider them like they are.  Other microservices will rely on these APIs and changes to them can drastically effect how the other microservices work so thinking carefully about your APIs is very important in a cloud native application.

Using REST APIs will also be helpful in supporting various types of clients.  The most robust cloud native applications today support desktops, browsers, phones, tablets, watches, TVs, cars, entertainment systems, game consoles, and may other types of clients.  The protocol that works best for all these types of devices is REST.

With that said, in certain types of situations other protocols may be preferred.  This is fine, if it makes sense for your use case, but it typically is for internal communication between services and not external communication with clients.

&nbsp;

## Antifragile

[<img class="alignnone size-full wp-image-1063" src="http://ryanjbaxter.com/wp-content/uploads/2015/07/cropped-please-mishandle.jpg" alt="cropped-please-mishandle" width="960" height="250" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/07/cropped-please-mishandle-300x78.jpg 300w, http://ryanjbaxter.com/wp-content/uploads/2015/07/cropped-please-mishandle.jpg 960w" sizes="(max-width: 960px) 100vw, 960px" />](http://ryanjbaxter.com/wp-content/uploads/2015/07/cropped-please-mishandle.jpg)

Todays cloud native applications are expected to have 0 down time.  If Facebook goes down it is on the national news.  Will your application be subject to the same level of scrutiny or publicity?  Probably not, but I guarantee your application is just as important to your customer as Facebook is to the entire world so you can be sure you will hear for them if your application is not available.  The good news is that by following a microservices architecture you are already making sure your application is more robust.  A crash in one component should still leave the majority of your application available to your users.  If you take advantage of different data centers and regions from your cloud provider you have enough redundancy to withstand an entire outage of your application in one region without anyone even noticing!  There are other tools and patterns you can implement in your application that helps your application be even more robust and I will talk about some of these in future posts.