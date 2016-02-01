---
id: 1187
title: 'Building Cloud Native Apps With Spring  &#8211; Part 6'
date: 2015-10-22T13:46:50+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1187
permalink: /2015/10/22/building-cloud-native-apps-with-spring-part-6/
dsq_thread_id:
  - 4535002069
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
  - Spring
  - Tutorial
---
After wrapping up [part 5](http://ryanjbaxter.com/2015/10/12/building-cloud-native-apps-with-spring-part-5/) of this tutorial, we had a fairly well rounded, yet simple, microservices application able to run locally on our machine.  However, our ultimate goal is to not run the application locally but instead run it in the cloud, after all we are building a cloud native application.  My cloud of choice is IBM Bluemix.  However, the changes described in this post should work in any Cloud Foundry deployment, but I have only tested them on Bluemix.  (Spring Cloud is not bias to any cloud platform, however the way you configure and deploy your Spring Cloud applications may depend on the cloud you are targeting.)

The first step in getting our application to the cloud is not going to involve writing code, instead we are going to look at setting up an environment for maintaining and deploying our app that will allow us to automate as much as possible.  If our application is truley a [cloud native application, than it should also be a 12-factor application](http://ryanjbaxter.com/2015/07/13/building-cloud-native-applications/).  The first rule of being a 12-factor application is that our code should be in version control.  Another property of a 12-factor application is that they have strictly separate build, release, and run stages.  Luckily Bluemix provides <a href="https://hub.jazz.net/docs" target="_blank">DevOps services</a> integrated into the platform which provides us with a Git repo for version control and build and deploy tools where we can setup a build and deploy pipeline to build, release, and run our application on Bluemix.

We have one critical choice to make at this point before we get down and dirty, do we want to put all of our services in one Git repo or have a separate Git repo for each service?  When I first thought about what to do, I wanted to put everything in one Git repo just to simplify things.  However, after heading down that path, I realized that it was the wrong decision and instead decided placing each service in its own Git repo was the right way to go.  While initially this will require more setup time, it allows each service to evolve independently of each other in a true microservice and cloud native fashion.  Take for example if we need to create a branch to work on a defect for one service, we don&#8217;t want a branch for all the other services as well, the only way to do this is if each service is in its own Git repo.

In this post we are going to walk through how to deploy our Eureka server to the cloud since it is somewhat different than the other services in our application.  In the next post we will deploy the rest of the services.

## Create A Git Repo For Our Eureka Server

Lets get started by <a href="https://hub.jazz.net/docs/startproject/" target="_blank">creating a new project in IBM DevOps services</a>.  Head to <a href="https://hub.jazz.net/" target="_blank">jazzhub.com</a>, login, and click the Create Project button.  Give your project the name _ocr-eureka_.  Choose the option to _Create a new repository_, then select your location.  I suggest you either create a Git repo on Bluemix or on GitHub.  If you want to initialize the repository with a README and license file you can, I chose not to select that option.  You can also choose to make the project private if you want, and add Scrum development features, however that is not really important for this exercise.  You should check off _Make this a Bluemix project_ and then select the Region, Organization, and Space you will want to deploy the application to.  This step will be important later on when we setup our build and deploy pipeline.  Below are screenshots of the configuration for my project.

[<img class="alignnone size-full wp-image-1189" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-10.18.29-AM.png" alt="Screen Shot 2015-10-16 at 10.18.29 AM" width="1069" height="919" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-10.18.29-AM-300x258.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-10.18.29-AM-1024x880.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-10.18.29-AM.png 1069w" sizes="(max-width: 1069px) 100vw, 1069px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-10.18.29-AM.png)

[<img class="alignnone size-full wp-image-1188" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-9.16.05-AM.png" alt="Screen Shot 2015-10-16 at 9.16.05 AM" width="1076" height="981" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-9.16.05-AM-300x274.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-9.16.05-AM-1024x934.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-9.16.05-AM.png 1076w" sizes="(max-width: 1076px) 100vw, 1076px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-9.16.05-AM.png)

Finally click the Create button to create your new Git repo.

We now want to get our code into this new Git repo.  Open a terminal window and navigate to the directory containing the _ocr-eureka_ project.  (You should navigate to the root of the project, the folder containing your POM file.)  Before we do anything with Git we will want to create a _.gitignore_ file so we don&#8217;t add files we don&#8217;t want in our repo.  Using your favorite text editor create a file called _.gitignore_ and add the following content to it.

<pre class="font-size:15 lang:default highlight:0 decode:true">.settings
target
.classpath
.project</pre>

Once you have saved that file run the following set of commands.

<pre class="font-size:15 lang:default highlight:0 decode:true">$ git init
$ git add .
$ git commit -m "initial commit"
$ git remote add origin &lt;url to git repo on Bluemix&gt;
$ git push origin master</pre>

All we are doing above is initializing a local Git repo, adding all our code to it, committing it, adding our new remote Git repo, and than pushing all the code we committed locally to our remote Git repo in Bluemix.  This should be nothing new to you if you are familiar with Git.  Once the code has been committed to the remote Git repo in Bluemix, you can refresh the repository page in IBM DevOps Services and you should see all your code.

[<img class="alignnone size-full wp-image-1190" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-10.21.34-AM.png" alt="Screen Shot 2015-10-16 at 10.21.34 AM" width="855" height="349" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-10.21.34-AM-300x122.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-10.21.34-AM.png 855w" sizes="(max-width: 855px) 100vw, 855px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-10.21.34-AM.png)

## Configuring Eureka To Run In The Cloud

If you look at the _application.yml_ file for our Eureka Server you will notice that the hostname is hardcoded to be _localhost_.  Obviously we will need to change that when Eureka gets deployed to Bluemix.  Luckily this should be a simple change, but how will we know the right URL to use?

There are a number of solutions to figuring out the right URL to use.  One solution would be to set an environment variable to the URL for your Eureka server when you deploy the application to Bluemix and then use that environment variable in your _application.yml_ file.  However Bluemix (and Cloud Foundry) already provides us with an environment variable with the URL(s) of the application.  The _VCAP_APPLICATION_ environment variable, made available to the application by Bluemix (Cloud Foundry), has a _uris_ property containing an array of all the URIs mapped to the application. So, instead of creating our own environment variable we can just use the value in _VCAP_APPLICATION_ instead.  Below is a sample of what the _VCAP_APPLICATION_ environment variable looks like.

<pre class="font-size:15 lang:default decode:true ">"VCAP_APPLICATION": {
  "application_name": "ocr-eureka",
  "application_uris": [
   "ocr-eureka.mybluemix.net"
  ],
  "application_version": "a20e5183-2627-48af-90bf-d11e4248a2f8",
  "limits": {
   "disk": 1024,
   "fds": 16384,
   "mem": 1024
  },
  "name": "ocr-eureka",
  "space_id": "10d307d3-8f7e-4d5c-8d95-6e3785e90738",
  "space_name": "ryanjbaxter",
  "uris": [
   "ocr-eureka.mybluemix.net"
  ],
  "users": null,
  "version": "a20e5183-2627-48af-90bf-d11e4248a2f8"
 }
}</pre>

Open your _application.yml_ file for your Eureka server and copy the below YAML into your file.

<pre class="font-size:15 lang:default decode:true">server:
  port: 8761
  
eureka:
  instance:
    hostname: ${vcap.application.uris[0]:localhost}
  client:
    registerWithEureka: false
    fetchRegistry: false
    serviceUrl:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/</pre>

Some of you might not be familiar with the syntax used in the _hostname_ property.  The _${value}_ syntax is a placeholder that lets us reference environment variables.  Spring Boot does some magic when it is running in a Cloud Foundry environment to flatten out the JSON of the _VCAP_APPLICATION_ environment variable so we can reference values via the dot (.) syntax in the placeholder name.  The colon in-between _vcap.application.uris[0]_ and _localhost_ is kind of like an if else statement.  It basically says if the environment variable _VCAP_APPLICATION_ contains a _uris_ property use the first item in that array as the hostname value, if not use localhost.  By using this if-else syntax we can run our application locally or in the cloud without much effort (assuming you don&#8217;t have a _VCAP_APPLICATION_ environment variable set in your local dev environment).  You could use Spring Profiles to do the same thing, but this keeps our YAML file a little cleaner, so I like this syntax better.

Make sure you commit and push these changes to your remote Git repo in Bluemix.

## Setting Up A Build And Deploy Pipeline For Eureka

Now lets configure the build and deploy stages for our Eureka service on Bluemix.  Back in the the project on IBM DevOps Services click the Build and Deploy button.  Click the Add Stage button to add a new stage.  Give the stage the name _Build_.  For the Input Type, select _SCM Repository_, this should automatically populate the Git URL field.  Make sure _master_ is selected under the Branch drop down.  In the Stage Trigger section select _&#8220;Run jobs whenever a change is pushed to Git&#8221;_.

[<img class="alignnone size-full wp-image-1191" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.10.46-PM.png" alt="Screen Shot 2015-10-16 at 4.10.46 PM" width="765" height="706" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.10.46-PM-300x277.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.10.46-PM.png 765w" sizes="(max-width: 765px) 100vw, 765px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.10.46-PM.png)

&nbsp;

Next click on the Jobs tab.  Click the ADD JOB button and select _Build_.  Under the Builder Type select _Maven_.  The defaults for the build should be fine, we just need to make a small change to the shell script.  Our code is using Java 1.8 but by default the build will use Java 1.7.  To fix this problem change we need to change the JAVA_HOME environment variable to point to Java 1.8.

<pre class="font-size:15 lang:default highlight:0 decode:true">#!/bin/bash
export JAVA_HOME=~/java8
mvn -B package</pre>

&nbsp;

[<img class="alignnone size-full wp-image-1194" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.37.47-PM.png" alt="Screen Shot 2015-10-16 at 4.37.47 PM" width="761" height="773" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.37.47-PM-295x300.png 295w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.37.47-PM.png 761w" sizes="(max-width: 761px) 100vw, 761px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.37.47-PM.png)

Click Save to save the Build stage.

Back on the Build and Deploy page click the Add Stage button again to add another stage.  Give the stage a name of _Deploy_.  Under Input Type select _Build Artifacts._  By selecting Build Artifacts for the Input Type this stage will be provided with the build artifacts from another stage.  In the Stage and Job drop downs select _Build_.  Under stage trigger, select &#8220;_Run jobs when the previous stage is completed&#8221;_ so that our deploy stage will run automatically after the Build stage finishes.

[<img class="alignnone size-full wp-image-1192" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.19.27-PM.png" alt="Screen Shot 2015-10-16 at 4.19.27 PM" width="760" height="691" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.19.27-PM-300x273.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.19.27-PM.png 760w" sizes="(max-width: 760px) 100vw, 760px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.19.27-PM.png)

Next click the JOBS tab.  Click ADD JOB and select Deploy.  Under Deployer Type select _Cloud Foundry_.  Under Target pick the datacenter you want to deploy to.  Then select the Organization and Space you want to deploy the application to.  We will need to modify the deploy script slightly to point at the Jar file we want to deploy.  In addition to adding the Jar to deploy you probably will also want to configure the hostname for the Eureka server.  If you don&#8217;t you can run into failures when you deploy the application if the hostname is already in use.  Change the Deploy Script as follows.

<pre class="font-size:15 lang:default highlight:0 decode:true">#!/bin/bash
cf push "${CF_APP}" -p ocr-eureka-0.0.1-SNAPSHOT.jar -n ocr-eureka -m 512M</pre>

You will want to change the value of the _-n_ option in your script to be a unique hostname of your choosing.

[<img class="alignnone size-full wp-image-1193" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.36.22-PM.png" alt="Screen Shot 2015-10-16 at 4.36.22 PM" width="755" height="923" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.36.22-PM-245x300.png 245w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.36.22-PM.png 755w" sizes="(max-width: 755px) 100vw, 755px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-16-at-4.36.22-PM.png)

Click Save to return to the Build and Deploy screen.

Now you can test everything out by clicking the play button on the Build stage.  This will preform the build and then do the deployment to Bluemix.  If you would like to watch the Build and Deploy stages you can click the _View logs and history_ links to see realtime logs of what is happening during each stage.  After the deployment finishes the Deploy stage will contain a link to the running Eureka server.  Click it to make sure the app is running.  You should see the familiar Eureka UI.

[<img class="alignnone size-full wp-image-1204" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/2015-10-21-16_11_51.gif" alt="2015-10-21 16_11_51" width="1280" height="720" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/2015-10-21-16_11_51.gif)

&nbsp;

## Eureka User Provided Service

Our other services (Races, Participants, Web, and Hystrix/Turbine) will all need to use Eureka.  Up until this point we just hard coded the URL to the Eureka server in the _application.yml_ files of these services, however in the cloud we want to do something a little more resilient.  Bluemix (and Cloud Foundry) have a concept of services, somewhat different than a microservice, but along the same lines (the term service is overloaded these days).  These are things like Databases, message queues, etc.  Besides the services provided by the cloud platform, users can also create their own services for use in apps they deploy to the platform, these are called <a href="https://docs.cloudfoundry.org/devguide/services/user-provided.html" target="_blank">user-provided services</a>.  For our cloud deployment, we will create a user-provided service for our Eureka server that other apps (microservices) can then bind to.  This gives us a nice abstraction layer between our Eureka server and the apps using that server.  To create a user provided service we need to use the <a href="https://docs.cloudfoundry.org/devguide/installcf/" target="_blank">Cloud Foundry command line tool</a>.  Once you have the Cloud Foundry CLI installed, run the following command to authenticate with Bluemix and target the space and organization your Eureka server is deployed to.

<pre class="font-size:15 lang:default highlight:0 decode:true">$ cf login -a api.ng.bluemix.net</pre>

Then run the _cf cups_ command to create a user-provided service called _ocr-eureka_.

<pre class="font-size:15 lang:default highlight:0 decode:true ">$ cf cups ocr-eureka -p '{"uri": "http://ocr-eureka.mybluemix.net/eureka/"}'</pre>

Make sure you replace the hostname in the _uri_ property in the command above with the correct URI to your Eureka server.  IT IS VERY IMPORTANT THAT YOUR URI ENDs IN _&#8220;/eureka/&#8221;_.  If you don&#8217;t construct the URI properly your microservices will fail to register themselves with Eureka.

You should now be able to see the service in your Bluemix dashboard or if you do _$ cf services_ from the command line.

[<img class="alignnone size-full wp-image-1198" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-20-at-4.21.45-PM.png" alt="Screen Shot 2015-10-20 at 4.21.45 PM" width="300" height="283" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-20-at-4.21.45-PM.png)

In the next step in the tutorial we will look at how we deploy the other services in our app to Bluemix.

&nbsp;

&nbsp;