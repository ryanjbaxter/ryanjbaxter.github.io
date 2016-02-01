---
id: 1244
title: Client Libraries For IBM IoT Foundation
date: 2016-01-29T14:29:29+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1244
permalink: /2016/01/29/client-libraries-for-ibm-iot-foundation/
dsq_thread_id:
  - 4535200939
categories:
  - BlueMix
  - Cloud
---
If you are familiar with the <a href="https://internetofthings.ibmcloud.com/#/" target="_blank">IBM IoT Foundation</a>, than you know the protocol used for communication between apps and devices is <a href="https://docs.internetofthings.ibmcloud.com/reference/mqtt/index.html" target="_blank">MQTT</a>.  <a href="http://mqtt.org/" target="_blank">MQTT </a>is an open protocol ideal for IoT solutions due to its lightweight nature.  It works very well when used by devices that might be running on a battery or my have a low bandwidth connection.  In the past when I have worked with the IBM IoT Foundation I have always used an <a href="https://github.com/mqtt/mqtt.github.io/wiki/libraries" target="_blank">MQTT library from mqtt.org</a>.  These libraries work fine with the IBM IoT Foundation as long as they support MQTT 3.1.  However the IBM IoT Foundation offers its own set of client libraries for both <a href="https://docs.internetofthings.ibmcloud.com/devices/mqtt.html" target="_blank">devices</a> and <a href="https://docs.internetofthings.ibmcloud.com/applications/mqtt.html" target="_blank">applications</a> that make using the IBM IoT Foundation a little bit easier for developers.  Currently there are client libraries for Node.js, Java, Python, C#, Embedded C, and mBedC.  So what is the advantage of using the client libraries provided by the IBM IoT Foundation over the client libraries directly from mqtt.org?  One advantage that I am a huge fan of is simplicity of code!  Lets take a look.

Here is some sample code that will allow an application to connect to the IBM IoT Foundation Quickstart service using the open source MQTT Node.js library directly.

<pre class="font-size:15 lang:default decode:true">var mqtt = require('mqtt');
var clientId = 'a:quickstart:demoapp';
var messagingPort = 1883;
var messagingHost = 'quickstart.messaging.internetofthings.ibmcloud.com';
var client = mqtt.createClient(messagingPort, messagingHost, {"clientId" : clientId});
client.on('connect', function() {
  console.log('connected to iot cloud');
});</pre>

Here is some code that does the same thing as above but uses the IBM IoT Foundation Node.js client library.

<pre class="font-size:15 lang:default decode:true">var Client = require('ibmiotf').IotfApplication;
var config = {
  "id" : "demoapp",
  "org" : "quickstart"
};
var app = new Client(config);
app.connect();
app.on('connect', function() {
  console.log('connected to ibm iotf');
});</pre>

Yes the IBM IoT Foundation client library code is 2 lines longer than the MQTT client library, but number of lines is not the only measure of simplicity.  Look at what I didn&#8217;t need to know when using the IBM IoT Foundation client library.  I didn&#8217;t need to know the URL or port to the Quickstart service MQTT broker, and I didn&#8217;t need to know how to construct the client id.  I don&#8217;t know about you, but whenever I need to connect to the IBM IoT Foundation I am always searching through documentation to find the right MQTT broker URL and how to construct the client ID properly.  The IBM IoT Foundation client library is even a bigger help when connecting to your own organization in the IBM IoT Foundation.  When using your own organization you need to authenticate with an API key and token as well.  So if you are not already using the IBM IoT Foundation client libraries in your IoT apps I suggest you start doing so soon!