---
id: 1144
title: Using Docker To Deploy Bluechatter To Bluemix
date: 2015-09-24T14:33:13+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1144
permalink: /2015/09/24/using-docker-to-deploy-bluechatter-to-bluemix/
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - Docker
  - PaaS
---
This week I gave a <a href="http://www.meetup.com/Bluemix-Developers-in-Cambridge/events/224699329/" target="_blank">presentation at the local Bluemix meetup group</a> on Docker containers and Bluemix.

[<img class="alignnone size-full wp-image-1145" src="http://ryanjbaxter.com/wp-content/uploads/2015/09/highres_147421062.jpeg" alt="highres_147421062" width="360" height="239" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/09/highres_147421062-300x199.jpeg 300w, http://ryanjbaxter.com/wp-content/uploads/2015/09/highres_147421062.jpeg 360w" sizes="(max-width: 360px) 100vw, 360px" />](http://ryanjbaxter.com/wp-content/uploads/2015/09/highres_147421062.jpeg)

For the demonstration during the presentation I decided I would use one of the apps <a href="http://ryanjbaxter.com/2014/04/22/bluechatter-a-sample-app-for-bluemix/" target="_blank">I first wrote as a Bluemix developer advocate</a>, <a href="https://github.com/IBM-Bluemix/bluechatter" target="_blank">Bluechatter</a>.  Bluechatter is a very basic IRC/chat like application written in Node.js.  It uses the pub/sub feature of Redis to distribute chat messages amongst various instances of the application, so it can scale very well.  For the meetup demonstration I wanted to show how you can deploy the Bluechatter application as a Docker container in Bluemix.

[<img class="alignnone size-full wp-image-1043" src="http://ryanjbaxter.com/wp-content/uploads/2015/07/bluemix_docker.png" alt="bluemix_docker" width="1100" height="550" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/07/bluemix_docker-300x150.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/07/bluemix_docker-1024x512.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/07/bluemix_docker.png 1100w" sizes="(max-width: 1100px) 100vw, 1100px" />](http://ryanjbaxter.com/wp-content/uploads/2015/07/bluemix_docker.png)

To do this I added both a <a href="https://github.com/IBM-Bluemix/bluechatter/blob/master/Dockerfile" target="_blank">Dockerfile</a> and a <a href="https://github.com/IBM-Bluemix/bluechatter/blob/master/docker-compose.yml" target="_blank">Docker Compose YAML file</a> to the Bluechatter repository on GitHub.

If you would like to run Bluechatter in a Docker container on your local machine, I suggest you use Docker Compose, as it will start both a container running Redis and a container running Bluechatter.  See the <a href="https://github.com/IBM-Bluemix/bluechatter/blob/master/README.md#running-in-a-docker-container-locally" target="_blank">README</a> for instructions on how to do this.  If you want to start Bluechatter as a Docker container running in Bluemix than you have two options.  The first and easiest option is to use the Deploy To Bluemix button.  Clicking the button in the <a href="https://github.com/IBM-Bluemix/bluechatter/blob/master/README.md#deploying-to-bluemix" target="_blank">README</a> (or just click <a href="https://bluemix.net/deploy?repository=https://github.com/IBM-Bluemix/bluechatter" target="_blank">here</a>!) will setup a Git repo in IBM DevOps Services as well as configure a deployment pipeline to deploy the application as both a Cloud Foundry application and a Docker container on Bluemix.  Your second option is to deploy a Bluechatter Docker image to Bluemix using the CLI.  Again the <a href="https://github.com/IBM-Bluemix/bluechatter/blob/master/README.md#running-the-container-on-bluemix" target="_blank">README</a> has instructions on how to do that as well.

So if you are looking for a sample app to get started with Docker containers on Bluemix check out Bluechatter, it is simple, strait forward, and easy to build upon!