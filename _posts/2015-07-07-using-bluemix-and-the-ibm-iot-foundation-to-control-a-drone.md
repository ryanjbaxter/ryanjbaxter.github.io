---
id: 1049
title: Using Bluemix and The IBM IoT Foundation To Control A Drone
date: 2015-07-07T13:41:09+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1049
permalink: /2015/07/07/using-bluemix-and-the-ibm-iot-foundation-to-control-a-drone/
categories:
  - BlueMix
  - Cloud
  - PaaS
---
Drones have been getting a lot of attention lately for both good and bad reasons.  Regardless of what the reason may be the bottom line is there are a lot of untapped uses for drones. Everything from <a href="http://www.amazon.com/b?node=8037720011" target="_blank">delivering your online packages</a> to <a href="http://www.shiftcentral.com/blog/drone-technology-could-have-future-emergency-response" target="_blank">helping first responders at the scene of an accident</a>.  Whatever the use case may be there needs to be a robust infrastructure behind these drones to manage them.  The cloud is an ideal choice in this use case because you will more than likely have many many drones and need a dynamic infrastructure to handle the varying needs of the drones and the applications controlling them.  Obviously Bluemix is my cloud of choice  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />but in addition to the infrastructure Bluemix provides we also have the <a href="https://internetofthings.ibmcloud.com/#/" target="_blank">IoT Foundation</a> which provides you with the ability to manage your connected devices.  I recently had the opportunity to work with some drones for a couple of weeks and decided to use Bluemix and IBM&#8217;s IoT foundation to see how you can control drones.  Check out the short video below of what I have done so far <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

&nbsp;

<div class="jetpack-video-wrapper">
  <span class='embed-youtube' style='text-align:center; display: block;'></span>
</div>

&nbsp;

Some of the code I showed working in the video is located in this <a href="https://github.com/IBM-Bluemix/parrot-ar-sample" target="_blank">GitHub repository</a>.  The important code in that repository is drone-controller.js which actually listens for command from the IoT Foundation and then sends them to the drone.  This code can easily be extended to accomplish many more things with the drone but right now it is a very basic sample.  Take a look through the README in the repository which explains a lot about what is happening behind the scenes.