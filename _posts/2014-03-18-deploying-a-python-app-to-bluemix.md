---
id: 668
title: Deploying A Python App To BlueMix
date: 2014-03-18T13:39:19+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=668
permalink: /2014/03/18/deploying-a-python-app-to-bluemix/
dsq_thread_id:
  - 4535001779
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
---
For the past 2 days I have been trying to get a very simple Python app deployed and running on BlueMix.  This proved to be a little challenging because there were no clear cut instructions or simple examples to go by (that I could find at first), and I had never written any Python before.  After spending most of the day yesterday talking to some expert IBMers I eventually got a simple hello world Python app using <a href="http://flask.pocoo.org/docs/" target="_blank">Flask</a> deployed to BlueMix.  Later on, I came across this <a href="https://github.com/michaljemala/hello-python" target="_blank">project on GitHub</a> which covers everything I am about to tell you and would have saved me two days of aggravation, but that is software development for you.  Here are the three &#8220;gotchas&#8221; that I ran into over the past few days, hope this saves others some time.

  1. You need a requirements.txt file in the root of your project.  The requirements.txt file should list all the dependencies that need to be installed in order to run your application.  When your application is staged the platform will read this file and install all the dependencies before trying to run your application.  The details around the requirements.txt file can be found in the <a href="http://www.pip-installer.org/en/latest/user_guide.html#id8" target="_blank">pip user guide</a>.  It is useful to know that you can also use local dependencies as long as they are in the root of your application.  When you have local dependencies in the root of your application they will be uploaded, along with your application code, to BlueMix.  In your requirements.txt file you would reference them by path, for example, ./mylocaldep.  This points the platform to the dependency code you uploaded as opposed to looking for it in a repository.
  2. Use the right buildpack!  This one had me baffled for a while.  Actually it would have been easy to figure out if BlueMix was logging a little more information (this feature will be coming soon to BlueMix).  When looking at the <a href="https://github.com/cloudfoundry-community/cf-docs-contrib/wiki/Buildpacks" target="_blank">community buildpacks</a> I selected the Heroku one (<https://github.com/heroku/heroku-buildpack-python>)
  
    figuring it would be fairly stable.  DON&#8217;T DO IT!!!  Apparently this buildpack does not work on BlueMix or Cloud Foundry.  I eventually tried this <a href="https://github.com/joshuamckenty/heroku-buildpack-python" target="_blank">one</a>, and that worked.  I don&#8217;t know what the difference is between it and the Heroku one, it appears to be a fork.  (Later on I noticed the simple Python project I liked to on GitHub above also pointed out that the Heroku buildpack was not working.)
  3. Make sure you get the port from the environment variable <a href="https://github.com/michaljemala/hello-python/blob/master/hello.py#L10" target="_blank">VCAP_APP_PORT </a>and use it when starting your HTTP server.  If you don&#8217;t your app will fail to start.  This might be specific to the library I was using (Flask) although I imagine it would apply to any Python HTTP server.