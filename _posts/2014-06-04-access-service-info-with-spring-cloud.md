---
id: 763
title: Access Service Info With Spring Cloud
date: 2014-06-04T14:30:13+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=763
permalink: /2014/06/04/access-service-info-with-spring-cloud/
dsq_thread_id:
  - 4535335160
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
---
Adding services to your application to integrate additional functionality is one of the primary benefits of using a PaaS like Bluemix.  Whether that is a database, a mobile push service, or some type of analytics service, you just pick what you need and bind them to your application.  You don&#8217;t have to install software of setup VMs, just point and click.  After you have done that you are always faced with the same problem, you now need to access the service info in the VCAP\_SERVICES environment variable.  This info usually consists of some type of credentials or API keys, something that authorizes you to use the service in one way or another. As everyone probably knows, the VCAP\_SERVICES environment variable is a JSON object, so extracting information from it just involves parsing JSON.  In Java you have many choices for doing that which I will not go into here, but it is a fairly simple task.  If you are going to be using more than one service in your application your probably going to want to write some type of utility class which allows you to extract arbitrary info for different services by passing some type of unique identifier for the service (typically the service name).  This will return you more JSON to work with.  Again, none of this is hard stuff, even an entry level developer.  However it is kind of monotonous, and when you think about the fact you need to implement this same pattern for every application you deploy to Bluemix, it is unbearable. A helper library would make our lives as Java developers better.  Apparently I am not the only person to think this because one exists  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> It is called Spring Cloud.  Don&#8217;t let the name fool you, this library is not limited to just Spring apps, it can be used by any Java app, but if you are using Spring there are some added benefits.

Here is a great <a href="http://spring.io/blog/2014/06/03/introducing-spring-cloud?utm_content=buffer98369&utm_medium=social&utm_source=twitter.com&utm_campaign=buffer" target="_blank">article</a> describing the benefits of using Spring Cloud, it also contains a sample app with code to get you started.  The article references using the library with Cloud Foundry and Heroku, but since Bluemix is built upon Cloud Foundry it all works with Bluemix as well.  If it helps you when reading the article just replace the words &#8220;Cloud Foundry&#8221; with Bluemix  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> The GitHub repo for the project can be found <a href="https://github.com/spring-projects/spring-cloud" target="_blank">here</a>.  If you are a Java developer, I highly suggest you take advantage of this library.  I have use the library in both the <a href="https://github.com/CodenameBlueMix/todo-apps/tree/master/java" target="_blank">Java ToDo app</a> and the <a href="https://github.com/CodenameBlueMix/session-questions" target="_blank">Session Questions app</a>.