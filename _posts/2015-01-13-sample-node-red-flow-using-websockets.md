---
id: 957
title: Sample Node-RED Flow Using Websockets
date: 2015-01-13T15:42:41+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=957
permalink: /2015/01/13/sample-node-red-flow-using-websockets/
dsq_thread_id:
  - 4535001977
categories:
  - BlueMix
  - Cloud
---
I have been using <a href="http://nodered.org" target="_blank">Node-RED</a> as part of <a href="https://console.ng.bluemix.net/?ssoLogout=true#/store/cloudOEPaneId=store&appTemplateGuid=iot-template&fromCatalog=true" target="_blank">Bluemix</a> for a while now, it is super easy to build some pretty powerful applications by just dragging, dropping, and configuring &#8220;nodes&#8221; right in your browser.  90% of the time you don&#8217;t need to write any code, and when you do, the code is very simplistic.  I find Node-RED to be a great tool for getting something up an running quickly, for example when building a prototype or participating in a hackathon.  In a couple weeks I will be doing a <a href="http://sched.co/27sw" target="_blank">workshop</a> on Node-RED at <a href="http://developerweek.com/" target="_blank">DeveloperWeek</a> in San Francisco so I have been working on my slides and some examples for the workshop.  For one of my examples, I wanted to show how easy it was to interact with web services using Node-RED and thought that making it relate to San Francisco would make for a really nice example for the workshop.  I know <a href="http://www.bart.gov/" target="_blank">BART</a> (the subway system in San Franscisco) had some pretty robust APIs so I decided to check them out.  After a couple minutes of looking at the APIs I thought it would be nice if I could drop pins on a Google Map for all the BART stations in San Fran.  This has probably been done a million times before, but that wasn&#8217;t the point of this exercise, I wanted to show how easy it was to do in Node-RED.

The flow I came up with to accomplish this was to add a new HTTP endpoint called /mapstations and when that endpoint is called use a template node to respond back with some static HTML and JavaScript.  The JavaScript would load a Google Map and then make a request to a websocket to get the station data.  A websocket node in a separate Node-RED flow would then receive that request and make an HTTP request to the BART APIs to get the station data and respond back to the websocket request with that data.  The JavaScript would then take the station data, and using the GPS coordinates put a pin on the map for each station.  This ended up being surprisingly easy to do, and I thought it would be a good example to share.  I shared the flow on the Node-RED site and you can find it <a href="http://flows.nodered.org/flow/d7a45e7e693f43c9d47b" target="_blank">here</a>.  You can just copy the JSON from that page and import it into any Node-RED sheet.  Once it is deployed you can hit http://node-red-host/mapstations (replace node-red-host with the hostname for Node-RED) and you should see a Google Map with all the BART stations on it.  Enjoy!