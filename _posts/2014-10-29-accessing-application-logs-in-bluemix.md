---
id: 889
title: Accessing Application Logs In Bluemix
date: 2014-10-29T15:51:13+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=889
permalink: /2014/10/29/accessing-application-logs-in-bluemix/
dsq_thread_id:
  - 4535001905
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
---
<div class="pn-copy">
  <p>
    Being able to view your application’s log files is essential for any developer.  Developers use their log statements to gain insight into what is happening in their application throughout the application lifecycle.  Accessing your logs when running your application locally is easy, but when the application is deployed to a remote server things get a little bit trickier.  In the past, when deploying your app to your own infrastructure or even infrastructure-as-a-service (IaaS) you could access your like files via SSH or FTP.  With a platform-as-a-service (PaaS) things are much different, there is no way to SSH or FTP into the infrastructure running your app because that is all controlled by the platform.  In Cloud Foundry based PaaSes such as Bluemix, access to logs from an application are exposed via something called the<a href="http://docs.cloudfoundry.org/devguide/services/log-management.html#loggregator" target="_blank">Loggerator</a>.  The Loggerator is a fantastic piece of software, but luckily as a developer you don’t need to know anything about it except for the fact all the logs from your application are made available through it.  The most common way to access log files is via the Cloud Foundry command line interface (CLI).
  </p>
  
  <h3>
    $ cf logs
  </h3>
  
  <p>
    If you are going to be doing development on Bluemix, than I suggest you <a href="https://www.ng.bluemix.net/docs/#starters/BuildingWeb.html#install_cf" target="_blank">install the Cloud Foundry CLI</a> even if you are not a fan of the command line.  The tool is easy to use and there are plenty of <a href="http://docs.cloudfoundry.org/devguide/installcf/whats-new-v6.html" target="_blank">guides</a> out there that go through the details of using it.  In some cases the only option may be to use the CLI so it is a good tool to have installed.  One of the most useful commands in the CLI is <em>cf logs</em>.  <em>cf logs</em>allows you to access the logs from your application deployed to Bluemix right from your command line so it is extremely useful when doing development.  You can access a snapshot of recent logs or stream logs right to your command line.  Here are the basic uses.
  </p>
  
  <p>
    <em>cf logs appName –recent</em><br /> This command will show a subset of recent logs from the application deployed to Bluemix with the name appName.
  </p>
  
  <p>
    <em>cf logs appName</em><br /> This command will tail the logs from the application deployed to Bluemix with the name appName.  You will not be able to see any of the past logs with this command.
  </p>
  
  <p>
    Here is a short video describing how to use c<em>f logs</em>.
  </p>
  
  <p>
    <span class="youtube"></span>
  </p>
  
  <p>
    I generally use cf logs when doing development and testing, I almost always have a window open tailing the logs of the application I am working on.  Tailing the log files of your application is very useful when deploying applications to Bluemix.  Often times you may run into errors when doing deployments to Bluemix and it is not clear what went wrong.  Your first instinct might be to run <em>cf logs appName –recent</em>to see what went wrong.  Unfortunately you will notice that in most cases the commands exits without showing you any logs.  This happens because the file system is temporary, meaning that the contents of the file system (including your logs) are lost when the application is stopped, and since your application failed to start your logs don’t stick around for too long.  To solve this problem what I do when I deploy an application is use <em>cf logs appName</em> to tail the logs while the deployment is taking place.  This works as long as the application is created in Bluemix by the time you run the command.  For a quick demo of how to do this take a look at this video.
  </p>
  
  <p>
    <span class="youtube"></span>
  </p>
  
  <h3>
    Using Persistent Logging Services
  </h3>
  
  <p>
    Once your application is deployed and somewhat stable you will want to use a logging service.  There are a couple of reasons for wanting to use a logging service.  The first reason is using <em>cf logs appName –recent</em> will only allow you to see so far back in your logs.  The only way to view logs of your app over a long period of time is to use a logging service.  Second, as discussed above, logs are deleted after the application restarts, and restarts can happen as a normal corse of operation, especially if you are practicing continuous integration and delivery.  As with any type of cloud service you have a couple of options when it comes to logging services.
  </p>
  
  <h4>
    IBM’s Monitoring And Analytics
  </h4>
  
  <p>
    If you browse the Bluemix catalog you will come across the <a href="https://www.ng.bluemix.net/docs/#services/monana/index.html#gettingstartedtemplate" target="_blank">Monitoring and Analytics</a> service.  This service can be used with any app using the IBM Liberty or Node.js runtimes.  This service offers a variety of additional features besides log collection but log collection comes in the free plan.  With this service you can only view logs from the past 24 hours, so if you need your logs to be persisted longer than 24 hours consider one of the 3rd party services.  The nice thing about this service is that it offers great searching functionality so you can easily identify patterns in your logs.  Here is a short video that describing how to use the Monitoring and Analytics service.
  </p>
  
  <p>
    <span class="youtube"></span>
  </p>
  
  <h4>
    3rd Paty Logging Services
  </h4>
  
  <p>
    In addition to the Monitoring And Analytics service in the IBM Bluemix catalog, you are free to use any 3rd party logging service that supports the <a href="https://www.ng.bluemix.net/docs/#services/monana/index.html#gettingstartedtemplate" target="_blank">syslog protocol</a>.  This includes services such as <a href="http://docs.cloudfoundry.org/devguide/services/log-management-thirdparty-svc.html" target="_blank">Papertail, Splunk Storm, SumoLogic, and Logentries</a>.  To use one of these services you must first setup an account with one of them.  Most of them offer some level of functionality for free giving you an opportunity to try out each of them and pick the one you like best.  After picking a service to use, you will go through a process where you register the application with the service and it provides you with a syslog URL to use.  Once you have this URL the process of connecting the Loggerator in Bluemix to the 3rd party service is the same.  To do this you need to use the Cloud Foundry CLI to create whats called a <a href="http://docs.cloudfoundry.org/devguide/installcf/whats-new-v6.html#user-provided" target="_blank">user provided service</a> (this is one of those cases where you need to use the CLI to do this).  The command is simple, just run
  </p>
  
  <p>
    <em>cf cups <serviceName> -l <syslog url></em>
  </p>
  
  <p>
    In the above command, replace <serviceName> with any name you would like and <syslog url> with the URL you got from your 3rd party logging service.  The URL should begin with syslog://, for example here is an example of a URL from the Papertrail service.
  </p>
  
  <p>
    syslog://logs.papertrailapp.com:1234
  </p>
  
  <p>
    Using 3rd party logging services is pretty simple as you can see in this video.
  </p>
  
  <div class="pn-fluid-embed-wrap">
    <span class="youtube"></span>
  </div>
</div>

<div class="cf pn-post-tags-and-share">
</div>