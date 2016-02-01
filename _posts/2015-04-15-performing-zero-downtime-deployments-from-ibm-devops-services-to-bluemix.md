---
id: 1023
title: Performing Zero Downtime Deployments From IBM DevOps Services To Bluemix
date: 2015-04-15T17:57:33+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1023
permalink: /2015/04/15/performing-zero-downtime-deployments-from-ibm-devops-services-to-bluemix/
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
---
In my [previous blog post](http://ryanjbaxter.com/2015/04/09/building-and-deploying-maven-projects-to-bluemix-with-ibm-devops-services/ "Building and Deploying Maven Projects To Bluemix With IBM DevOps Services") I showed how to setup a deployment pipeline in IBM DevOps Services for a Maven project.  The problem with that pipeline was that while the deployment is happening your application is unavaialable.  Obviously in a production situation this is unacceptable, we need our application to be available 100% of the time (or as close to that number as possible).  Luckily Bluemix allows us to map the same route to multiple applications allowing us to take advantage of the Cloud Foundry Router to load balance requests between multiple applications.  This allows us to keep the old version of our application running while deploying the new version.  Once the new version is deployed requests to the route will be sent to both versions of the application.  After we have verified that the new version of the application is working we can delete the old version or if the new version is not behaving as anticipated we can delete the new version and direct all users back to the old version until we can fix the problem.  This process is referred to as <a href="https://www.ng.bluemix.net/docs/#manageapps/index-gentopic3.html#d2e1" target="_blank">blue/green deployments</a>.

Using the Cloud Foundry Command Line you can easily script this process and since the Cloud Foundry Command Line is what is used in the deployment pipeline in IBM DevOps Services it is possible to create a deploy script that does this process automatically.  Since the Cloud Foundry CLI is not specific to any type of application I was able to create a <a href="https://gist.github.com/ryanjbaxter/9ec2dd5de1b1279338bf" target="_blank">reusable script</a> that you can use to do blue/green deployments for your apps.  In addition to doing blue/green deployments the script will also take care of binding additional routes to your application, setting environment variables, and binding all the necessary services.  After the new version of the application is deployed the script will also delete the old version of the application (if this is the first time you are deploying the application than it will obviously skip this step).  In this simple version of the script we do not worry about testing the new version of the application before deleting the old version.  In theory though you could kick off some tests using something like Selenium to verify the new version before deleting the old version.  There are a number of environment variable the script relies on to do things like know which services to bind and which routes to create.  These are all detailed in the usage comments at the top of the script.

To use this script in the deploy stage of your pipeline on IBM DevOps Services just paste the following Bash script in the Deploy Script editor.

<pre class="font-size:15 lang:default decode:true ">#!/bin/bash
git clone https://gist.github.com/9ec2dd5de1b1279338bf.git deploy
/bin/bash deploy/bluegreen.sh
RESULT=$?

if [ $RESULT -ne 0 ]; then
    echo -e "${red}Executed failed or had warnings ${no_color}"
    exit $RESULT
fi 
echo -e "${green}Execution complete${no_label}"</pre>

The only environment variable you need to set in order to make the script work properly is the _ROUTES_ environment variable which will make sure the correct route is bound to your application when it is deployed.  However I think almost every deploy stage should probably at least look at setting _CF\_PUSH\_ARGS_ to specify things like the amount of memory you want allocated for your app.  If you are pushing a war or jar file than you will have to use _CF\_PUSH\_ARGS_ to pass the _-p_ parameter for the war or jar you want to push.

Below is a short video on how to setup your deploy stage to use the script.

&nbsp;

<div class="jetpack-video-wrapper">
  <span class='embed-youtube' style='text-align:center; display: block;'></span>
</div>

&nbsp;