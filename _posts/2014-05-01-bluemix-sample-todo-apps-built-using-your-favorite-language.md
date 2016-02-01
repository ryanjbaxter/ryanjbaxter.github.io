---
id: 736
title: 'BlueMix Sample: ToDo Apps Built Using Your Favorite Language'
date: 2014-05-01T15:37:04+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=736
permalink: /2014/05/01/bluemix-sample-todo-apps-built-using-your-favorite-language/
dsq_thread_id:
  - 4535001819
categories:
  - BlueMix
  - Cloud
  - PaaS
---
The ToDo app is one of the most popular samples to build when learning new languages and frameworks.  Only the Hello World sample is more popular.  While Hello World makes you feel accomplished, it generally leaves you with more questions than answers.  To really understand how things are working you need to build something a little more complex.

[<img class="alignnone size-full wp-image-737" src="http://ryanjbaxter.com/wp-content/uploads/2014/05/download.jpeg" alt="download" width="271" height="186" />](http://ryanjbaxter.com/wp-content/uploads/2014/05/download.jpeg)

Enter the ToDo app.  When I joined the team of BlueMix dev advocates one of the first discussions we had was around which sample apps we wanted to build.  There are the cool flashy demo apps that you see during a keynote and then there are the real world samples that developers actually learn from.  I was interested in the real world samples, and I immediately thought of one of my favorite websites, [todomvc.com](http://todomvc.com).  If you are not familiar with todomvc.com I suggest you check it out.  The general idea of the site is to teach developers about various JavaScript MVC frameworks.  They do this by implementing the same application, a ToDo app, in each of the various JavaScript frameworks.  I really liked the idea, so I suggested we build a ToDo app for a couple of the more popular languages that BlueMix supports, everyone agreed.  In true open source fashion we took one of the samples from todomvc.com, the Backbone sample, and implemented a backend for it in Java, Node.js, Python, Sinatra, and PHP.  Developers interested in using BlueMix can pick the ToDo app for the language they plan on using, deploy it, and then see how it works by looking at the code.

[<img class="alignnone size-full wp-image-738" src="http://ryanjbaxter.com/wp-content/uploads/2014/05/logo-icon.png" alt="logo-icon" width="330" height="330" srcset="http://ryanjbaxter.com/wp-content/uploads/2014/05/logo-icon-150x150.png 150w, http://ryanjbaxter.com/wp-content/uploads/2014/05/logo-icon-300x300.png 300w, http://ryanjbaxter.com/wp-content/uploads/2014/05/logo-icon.png 330w" sizes="(max-width: 330px) 100vw, 330px" />](http://ryanjbaxter.com/wp-content/uploads/2014/05/logo-icon.png)

In addition we wanted to integrate a service into the ToDo app, since knowing how to leverage services in BlueMix is very important.  We decided to add a database to store the ToDos in.  In fact each sample can either use Cloudant or Mongo DB to store ToDos.

[<img class="alignnone size-full wp-image-739" src="http://ryanjbaxter.com/wp-content/uploads/2014/05/download-1.jpeg" alt="download (1)" width="260" height="180" />](http://ryanjbaxter.com/wp-content/uploads/2014/05/download-1.jpeg) [<img class="alignnone size-full wp-image-740" src="http://ryanjbaxter.com/wp-content/uploads/2014/05/download-2.jpeg" alt="download (2)" width="320" height="158" srcset="http://ryanjbaxter.com/wp-content/uploads/2014/05/download-2-300x148.jpeg 300w, http://ryanjbaxter.com/wp-content/uploads/2014/05/download-2.jpeg 320w" sizes="(max-width: 320px) 100vw, 320px" />](http://ryanjbaxter.com/wp-content/uploads/2014/05/download-2.jpeg)

The source code for the ToDo apps can be found on <a href="https://github.com/CodenameBlueMix/todo-apps" target="_blank">GitHub</a>.  The READMEs for the various apps contain many more details about how to deploy the apps to BlueMix.  If you have other languages or frameworks you would like to see a ToDo app implemented with please let me know.