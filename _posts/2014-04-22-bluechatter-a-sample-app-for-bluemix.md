---
id: 719
title: 'BlueChatter: A Sample App For BlueMix'
date: 2014-04-22T13:27:10+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=719
permalink: /2014/04/22/bluechatter-a-sample-app-for-bluemix/
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
---
Yesterday I published a new sample application to our <a href="https://github.com/orgs/CodenameBlueMix/dashboard" target="_blank">Codename: BlueMix organization</a> on GitHub called <a href="https://github.com/CodenameBlueMix/bluechatter" target="_blank">BlueChatter</a>.  BlueChatter is a chat / IRC-like application for your browser.  You enter a user name and then you can send messages.  If someone else is also using the app they will instantly see whatever messages you send.  Obviously this is not going to replace Skype or your favorite IRC client, instead it is meant to demonstrate how to scale your application in BlueMix.

BlueChatter uses long polling in order to instantly receive messages as other users send them.  What happens is the client (the browser) sends a request to the server.  The server then holds on to that request until it receives a message.  The client may receive a response immediately or it may be a minute later before it receives a response.  It all depends on when the next message is received.  Once a response is received the client immediately sends another request to the server to wait for the next message.  This is pretty strait forward but what happens when you scale the application in the cloud?  When you scale an application in a PaaS like BlueMix you are essentially starting multiple instances of the same application.  Users will connect to any given instance at random.  So when this is the case how can the architecture BlueChatter is using work?  Users will be sending messages to separate servers!  The answer is to use a shared service, or a common service that all server instances will use.  In the case of BlueChatter we use Redis.  Redis provides a pubsub API that works nicely for our use case.  When one server receives a new message it publishes the message to the Redis service.  All the server instances are also listening to the same topic using the Redis service.  So as soon as one server publishes a message to Redis all other servers will get notified and can then notify their clients of the message!

I have created a short video demonstrating the application.  Feel free to check out the code, I hope you find it useful!

<span class="youtube"></span>