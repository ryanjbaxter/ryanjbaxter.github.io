---
id: 552
title: Getting A List Of All Services On BlueMix
date: 2014-03-05T09:51:10+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=552
permalink: /2014/03/05/getting-a-list-of-all-services-on-bluemix/
dsq_thread_id:
  - 4535001776
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
---
One of the best features of Cloud Foundry and BlueMix is the services available for developers to integrate into their apps.  The first step in using any of these services is creating them.  You can create them in various ways, through the BlueMix UI, with the command line, with the Cloud Foundry Maven plugin, and with a manifest.yml file.  In almost every situation where you are creating the service you need will need to know the name and the plan.  Up until know I have been figuring out this information by looking through the BlueMix documentation but today I <a href="https://www.ibmdw.net/answers/questions/8616/is-there-a-single-place-to-see-all-the-service-plans-at-once/" target="_blank">found out</a> about the _cf marketplace_ command that will give you details about all the services on BlueMix right in your command line!  Here is what the output looks like after executing the command

&nbsp;

> $ cf marketplace
> 
> Getting services from marketplace in org myorg / space dev as user&#8230;
> 
> OK
> 
> &nbsp;
> 
> service                  plans                   description
> 
> BLUAcceleration          Community_Beta          BLU Acceleration service provides a powerful, easy-to-use, and agile platform for business intelligence and analytics. This enterprise-class managed-service is powered by the in-memory optimized, column-organized BLU Acceleration data warehousing technology.
> 
> Cloud Integration        user-provided           Securely connect and integrate applications and information in the cloud. Use patterns for accelerated integration or develop custom APIs as needed.
> 
> CloudCode                cloudcodeservice-free   Run server side scripting logic on the IBM cloud without having to worry about managing application servers or scale.
> 
> Cloudant                 user-provided           Cloudant’s distributed database as a service (DBaaS) allows developers of fast-growing web and mobile apps to focus on building and improving their products, instead of worrying about scaling and managing databases on their own. Highly available, durable and feature-rich, our data store is built for scaling, optimized for concurrent reads & writes, and handles a wide variety of data types including JSON, full-text, and geospatial.
> 
> DataCache                free                    Improve the performance & user experience of web applications by retrieving information from fast, managed, in-memory caches, instead of relying entirely on slower disk-based databases.