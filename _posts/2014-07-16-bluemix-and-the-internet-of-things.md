---
id: 820
title: Bluemix and the Internet of Things
date: 2014-07-16T15:00:21+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=820
permalink: /2014/07/16/bluemix-and-the-internet-of-things/
dsq_thread_id:
  - 4535001856
categories:
  - BlueMix
  - Cloud
  - PaaS
---
Update:  Some of the content here is out of date, please see my [updated blog post](http://ryanjbaxter.com/2014/11/12/updated-bluemix-and-the-internet-of-things/ "Updated: Bluemix and the Internet of Things").

&nbsp;

Want to take a guess as to how many devices will be connected to the internet by the end of 2015?  Give up?  15 billion.  Yup ,that&#8217;s right, that is 15 followed by 9 zeros.  By 2020 that number will increase to 40 billion!

<a href="http://ryanjbaxter.com/wp-content/uploads/2014/07/internet-of-things-connected-devices.png" target="_blank" rel="http://www.silabs.com/products/pages/internet-of-things.aspx"><img class="alignnone wp-image-821 size-full" src="http://ryanjbaxter.com/wp-content/uploads/2014/07/internet-of-things-connected-devices.png" alt="internet-of-things-connected-devices" width="700" height="270" /></a>

All of these devices make up what is called the internet of things.  As you might imagine with the number of devices growing there have been a number of new technologies emerging that help developers connect and use the data comming from these devices.  One of the more important ones is <a href="http://mqtt.org/" target="_blank">MQTT</a>.  &#8220;<span style="font-style: italic; color: #000000;">MQTT is a machine-to-machine (M2M)/&#8221;Internet of Things&#8221; connectivity protocol. It was designed as an extremely lightweight publish/subscribe messaging transport.&#8221;  </span>In response to the growing number of devices and the clear importance of gathering information from these devices IBM has developed an Internet of Things (IoT) Cloud which at the core has an MQTT instance that developers can use.  This offering is currently in beta but is quite functional and can be used to publish and retrieve data from your connected devices. Of course once you have a device connected to the IBM IoT Cloud you want to build an app around the data coming from that device.  This is where Bluemix comes in.  Using a combination of the IBM IoT Cloud and IBM Bluemix you can have a complete cloud based solution for your IoT apps.  Bluemix already has a service that is part of its catalog to connect to the IBM IoT Cloud.  In addition to the service, Bluemix has a boilerplate for the internet of things which stands up a Node-RED instance allowing you to design flows for your IoT data.

[<img class="alignnone wp-image-826 size-thumbnail" src="http://ryanjbaxter.com/wp-content/uploads/2014/07/Screen-Shot-2014-07-15-at-9.18.37-AM-150x150.png" alt="Screen Shot 2014-07-15 at 9.18.37 AM" width="150" height="150" />](http://ryanjbaxter.com/wp-content/uploads/2014/07/Screen-Shot-2014-07-15-at-9.18.37-AM.png) [<img class="alignnone wp-image-827 size-thumbnail" src="http://ryanjbaxter.com/wp-content/uploads/2014/07/Screen-Shot-2014-07-15-at-9.18.54-AM-150x150.png" alt="Screen Shot 2014-07-15 at 9.18.54 AM" width="150" height="150" />](http://ryanjbaxter.com/wp-content/uploads/2014/07/Screen-Shot-2014-07-15-at-9.18.54-AM.png)

As part of IBM&#8217;s IoT Cloud there are a number of &#8220;<a href="https://developer.ibm.com/iot/recipes/" target="_blank">recipes</a>&#8220;.  Recipes are basically example IoT scenarios that can get you up and running quickly.  The recipes contain instructions on how to setup the device as well as provide you with the code needed to connect the device to the IoT Cloud and publish data.  What makes using these recipes even easier is the <a href="http://quickstart.internetofthings.ibmcloud.com/#/" target="_blank">Quickstart service</a> that is part of the IoT Cloud.  When using the IoT Quickstart you don&#8217;t have to sign up for anything or register any devices, you just run the code given to you in the recipe, head to the qucikstart page, enter the MAC address of the device, and you can see the data being published to the IoT Cloud.  There are some limitations to using the Quickstart, but to get a device up and running quickly it couldn&#8217;t be easier. Being new to the whole IoT concept I decided to pick one of the recipes to experiment with.  I ended up choosing the <a href="https://developer.ibm.com/iot/recipes/ti-beaglebone-sensortag/" target="_blank">Beaglebone Black and TI Sensor Tag recipe</a>.  The main reason I chose this one was because the Sensor Tag had a number of sensors on it that you could use for different and interesting use cases.  Plus the Beaglebone Black, Sensor Tag, and Bluetooth LE USB adapter costs less than $100.  After my Beaglebone Black and Sensor Tag came in I was able to quickly follow the instructions in the recipe to start publishing data to the IoT Cloud.

Of course like any good developer I started to look at the <a href="https://github.com/ibm-messaging/iot-beaglebone" target="_blank">code</a> provided in the recipe.  I noticed that what it was doing was gathering data from all the sensors on the Sensor Tag and then publishing it under a single topic to the IoT Cloud.  I thought it would be better to publish multiple topics so consumers can subscribe to just the data they need.  I also wanted to use the left and right buttons on the Sensor Tag and I thought you would see a lag between the time a button was pressed and the time the data was published to the IoT Cloud if you had to wait to gather data for all the sensors first.  I quickly realized that if you wanted to publish more than one topic to the IoT Cloud you could not use the Quickstart.  You actually had to sign up for an account and register the device.  This ended up being really easy as well, given there was a <a href="https://developer.ibm.com/iot/recipes/improvise-registered-devices/" target="_blank">recipe</a> describing how to connect a registered device to the IoT Cloud.   After some time coding I was left with 1 project with three components to it.

  * The first component publishes data from the Sensor Tag to the IBM IoT Cloud and is written in Node.js.  It publishes multiple topics for the different types of data coming from the Sensor Tag and requires you to register the device publishing the data.
  * The second component consumes the data from the Sensor Tag that was published to the IoT Cloud.  It contains 2 web apps, one which visualizes the values of the sensors and another which allows you to control an HTML5 slide deck with a Sensor Tag.  The server portion of the webapp is written in Node.js.  The Node.js code connects to the IoT Cloud and subscribes to the topics from the first component (the publish component).  When the Node code receives data from the IoT Cloud it published that data via web sockets to a web application.  Since this component is just a Node.js webapp it can easily be deployed to Bluemix.
  * The third component also consumes the data from the Sensor Tag but it consumes it via a Node-RED flow.  Again you can run this Node-RED flow in Bluemix.

The code for this project can be found on <a href="https://github.com/IBM-Bluemix/iot-sensor-tag" target="_blank">GitHub</a>.  The READMEs contain many more technical details.  Below are two videos demoing the code.  The first one is a short video demoing one single use case.  The second video goes more in depth and demos using the Beaglebone Black.

<span class="youtube"></span>

<span class="youtube"></span>

&nbsp;