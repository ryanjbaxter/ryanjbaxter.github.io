---
id: 838
title: Bluemix ToDo App Updates
date: 2014-07-18T15:55:55+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=838
permalink: /2014/07/18/bluemix-todo-app-updates/
categories:
  - BlueMix
  - Cloud
  - PaaS
---
The <a title="BlueMix Sample: ToDo Apps Built Using Your Favorite Language" href="http://ryanjbaxter.com/2014/05/01/bluemix-sample-todo-apps-built-using-your-favorite-language/" target="_blank">ToDo apps for Bluemix</a> was one of the first sample projects I published after becoming a developer advocate for Bluemix.  Even though the apps are simple, I still think they are some of the most meaningful samples we have.  Today I have made them even better 😉  Since Bluemix went GA we introduced the MongoLab service and an enhanced Cloudant service.  I have decided to change the ToDo apps so they take advantage of both of these new services.

Why change something that is already good?  The old community Mongo service was unreliable, it sometime lost connections with the app, and it was so popular that it was often running out of space to create new DBs.  The old Cloudant service was essentially a user-provided service.  This meant you had to go out to Cloudant signup for an account, create a database, and get an API Key and Token before you could even deploy a ToDo app that used Cloudant.  This was a huge pain.

With the new MongoLab service you get a reliable, production quality Mongo DB  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> Plus you also have the benefit of using the MongoLab console to see the contents of your DBs.

The new Cloudant service now automatically generates an API Key and Token for you when you create the service.  No more having to leave Bluemix to signup for Cloudant and all the ugly stuff <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

I have updated the <a href="https://github.com/IBM-Bluemix/todo-apps" target="_blank">Bluemix ToDo apps</a> (all languages) to use both of these new services.  Hopefully this makes something that was already good even better, enjoy!