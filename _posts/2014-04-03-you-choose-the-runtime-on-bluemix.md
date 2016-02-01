---
id: 704
title: You Choose The Runtime On BlueMix!!!
date: 2014-04-03T13:53:23+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=704
permalink: /2014/04/03/you-choose-the-runtime-on-bluemix/
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
---
As most of you know, you can deploy an app that uses virtually any runtime to BlueMix.  As long as there is a <a href="https://github.com/cloudfoundry-community/cf-docs-contrib/wiki/Buildpacks" target="_blank">buildpack</a> for the runtime, your app will run on BlueMix.  Of course knowing the ins and outs of using the buildpack for your runtime can be a little tricky sometimes.  That is why over the past few weeks I have been working on implementing a ToDo application in Java, Node, Sinatra, Python, and PHP.  This idea was inspired by <a href="http://todomvc.com/" target="_blank">todomvc.com</a> which is dedicated to showing developers how to use various client side JavaScript frameworks by implementing the same ToDo app.  The ToDo apps we have built have the same goal, but instead each app uses the same client side framework (JQuery and Backbone) while implementing the server side code in various languages.  As I am sure you can tell we just took the client side framework right from the todomvc.com <a href="http://todomvc.com/architecture-examples/backbone/" target="_blank">Backbone example</a>.  The source code should be open sourced soon, but until then here is a quick demo.

If you can, watch the video in 720p for best quality.

<span class="youtube"></span>