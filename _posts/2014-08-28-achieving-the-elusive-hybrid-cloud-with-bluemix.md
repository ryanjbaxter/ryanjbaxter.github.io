---
id: 859
title: Deploying Your Hybrid Cloud Apps Has Never Been Easier With Bluemix
date: 2014-08-28T11:57:53+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=859
permalink: /2014/08/28/achieving-the-elusive-hybrid-cloud-with-bluemix/
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
---
Public clouds are great.  They offer great elasticity and cost savings, and in the case of a PaaS like Bluemix, allows us (developers) to forget about maintaining any infrastructure and focus in on our code.  However we all realize that public cloud can&#8217;t be used for everything.  There are some applications and services that must remain in our own data centers on private networks.  The reasons why vary, maybe it is too much work to move the resource to the cloud, or maybe the data in the resource is too sensitive to risk exposing outside of your firewall.  Whatever the reason may be, you are bound to come across a scenario where you need to access private resources in your public cloud applications.  What are your options?  One option is to try to setup some type of VPN and create a secure connection between the public cloud and your private network.  This is probably the route to take if you are using an IaaS provider like SoftLayer to run your application.  With a PaaS this is not possible, you don&#8217;t have access to the machine your application is running on or the network it is using, so if you choose Bluemix to deploy your application what are your options?

Luckily Bluemix has a solution for this problem and it is called <a href="https://www.ng.bluemix.net/docs/#services/CloudIntegration/index.html#gettingstartedwithcloudintegation" target="_blank">Cloud Integration</a>.  The Cloud Integration service uses something called secure connectors to talk securely to your applications running behind the firewall.  These secure connectors run behind your firewall and establish a tunnel to the Cloud Integrations service (or Cast Iron Live).  The key here is that it is the secure connector is establishing the connection to the Cloud Integration service in Bluemix and not the other way around (this works on the premise that your servers running behind the firewall can talk to servers running in the cloud while servers in the cloud cannot talk to servers running behind your firewall.)  Once the Cloud Integration service knows about your server via the secure connector and a tunnel is established, your Bluemix app can use the Cloud Integration service to access data from your server behind the firewall.  Out of the box the Cloud Integration service has secure connectors for <a href="https://www.ng.bluemix.net/docs/#services/CloudIntegration/index.html#cloudint_create_dbapi" target="_blank">DB2, Oracle</a>, and <a href="https://www.ng.bluemix.net/docs/#services/CloudIntegration/index.html#cloudint_create_sapapi" target="_blank">SAP</a>, however you can use <a href="https://www.ng.bluemix.net/docs/#services/CloudIntegration/index.html#cloudint_createapi_CIliveorchestrations" target="_blank">Cast Iron Live</a> to connect to a wide range of services on a private network.

I make it sound very simple above, and it is, but your need to understand how everything works to make it click  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> Below is a number of videos walking through how you can use the Cloud Integration service to expose REST APIs behind your firewall to an app running in Bluemix.  I chose REST APIs because it is something almost every developer is familiar with and has not ties to a specific language or stack.  Anyone can write REST APIs in their favorite language to expose the data the need to their cloud based applications.

# Creating A Secure Connector

<span class="youtube"></span>

# Creating Your Orchestrations

<span class="youtube"></span>

# Using Your Cloud Integration APIs In A Bluemix App

<span class="youtube"></span>

&nbsp;