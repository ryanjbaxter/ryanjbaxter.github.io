---
id: 141
title: Bridging The Gap Between Plugins And Web Apps
date: 2010-12-27T19:58:35+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.wordpress.com/?p=141
permalink: /2010/12/27/bridging-the-gap-between-plugins-and-web-apps/
jabber_published:
  - 1293479915
  - 1293479915
email_notification:
  - 1293479916
  - 1293479916
categories:
  - Eclipse
  - Lotus
  - Notes
  - Plugin Development
---
Even since I have become involved with plugin development and Notes I have heard people say, I don&#8217;t want to write plugins I want to write a web app to do my integration.  When asked why, the answer is generally that they know how to write HTML, Javascript, and CSS, and they have no idea how to even get started with plugin development.  Even if they are willing to look into it, people often become frustrated and end up thinking plugin development is a black art.  As much as I try to teach people about plugin development, in the end some integration is probably best done via a web application.  Obviously there are limits to how much integration you will be able to do in Notes if you decide to use a web app but you can do some.  Notes/Expedior are very web app friendly.  Did you know that there is a web server in Notes?  Well there is and you can build web applications that run on that server.  In the end though, these web applications are just plugins which are using an SWT Browser control to render a web page.  That web page could be running locally on the internal web server in Notes or it may be running externally on some other server in the cloud.  Often times it is hard to bridge the gap between your plugin code and the web application which the plugin is rendering.  However, in 8.5.2 there is a new class which you can use that solves this problem.  The class is called BrowserFunction.  Bob Balfe <a href="http://blog.balfes.net/?p=1593" target="_blank">blogged about it</a> a while ago.   This class allows the javascript of your web app to call the BrowserFunction class in your plugin.  Here is a quick <a href="https://gist.github.com/756479" target="_blank">example</a> of a view part using this class.  If you would like to run this code, you can easily put this view part in a sidebar in Notes and give it a shot.