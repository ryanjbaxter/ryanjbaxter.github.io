---
id: 732
title: BlueMix / Cloud Foundry Manifest Generator
date: 2014-04-30T14:39:52+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=732
permalink: /2014/04/30/bluemix-cloud-foundry-manifest-generator/
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
---
Using [manifests](http://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html) when deploying your applications to BlueMix or any Cloud Foundry deployment makes your deployments simple and consistent.  They are a requirement for using the deployment features in the [IBM Dev Ops services](http://ryanjbaxter.com/2014/04/21/devops-services-for-bluemix/ "DevOps Services For BlueMix") but have many benefits even when doing development locally.  If you have very robust cf push commands it is much simpler to put all the parameters in a manifest so you don&#8217;t have to type them out each time.  The problem is that for newbies and people who are not familiar with YAML, writing a manifest can take a little effort, not because it is hard but because the syntax is tricky.  I got tired of having to go to the Cloud Foundry documentation each time I wanted to create a manifest so I wrote a simple <a href="http://cfmanigen.mybluemix.net/" target="_blank">web app</a> that will do it for you.  Using the web app you just fill out the form and your manifest is generated for you.  You can preview the manifest by clicking the Preview button and once you are satisfied with it you can click the Download button the save the file to your machine.  There are even tooltips for each field you need to fill out.  Hopefully this helps everyone else as much as it helps me <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

The source code for the application can be found on <a href="https://github.com/CodenameBlueMix/cf-manifest-generator" target="_blank">GitHub</a>.  If you want to add any enhancements feel free to submit a pull request.

&nbsp;

[<img class="alignnone wp-image-733 size-full" src="http://ryanjbaxter.com/wp-content/uploads/2014/04/Screen-Shot-2014-04-30-at-10.36.14-AM.png" alt="Screen Shot 2014-04-30 at 10.36.14 AM" width="739" height="846" srcset="http://ryanjbaxter.com/wp-content/uploads/2014/04/Screen-Shot-2014-04-30-at-10.36.14-AM-262x300.png 262w, http://ryanjbaxter.com/wp-content/uploads/2014/04/Screen-Shot-2014-04-30-at-10.36.14-AM.png 739w" sizes="(max-width: 739px) 100vw, 739px" />](http://cfmanigen.mybluemix.net/)