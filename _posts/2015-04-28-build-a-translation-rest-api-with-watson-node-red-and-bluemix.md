---
id: 1031
title: Build A Translation REST API With Watson, Node-RED, and Bluemix
date: 2015-04-28T13:17:44+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1031
permalink: /2015/04/28/build-a-translation-rest-api-with-watson-node-red-and-bluemix/
dsq_thread_id:
  - 4535017744
categories:
  - BlueMix
  - Cloud
  - PaaS
---
Today I was looking at some of the new <a href="http://jamesthom.as/blog/2015/04/22/ibm-watson-nodes-for-nodered/" target="_blank">Watson nodes for Node-RED created by my colleague James Thomas</a> and was presently surprised how easy it was to start using the Watson services in Node-RED.  (I am not sure why, I was surprised everything in Node-RED seems to be easy)  For one of my upcoming presentations I put together a simple flow for a REST API that uses the <a href="http://www.ibm.com/smarterplanet/us/en/ibmwatson/developercloud/machine-translation.html" target="_blank">Watson Machine Translation service</a> in order to translate text POSTed to an API endpoint.  Below is a video I created of how easy this was to do.

&nbsp;

<iframe width="420" height="315" src="https://www.youtube.com/embed/N6LZjswqD4U" frameborder="0" allowfullscreen></iframe>

&nbsp;

If you are interested in adding the flow to your Node-RED instance copy the below JSON and import into a sheet.  Note: you will need a Watson Machine Translation service bound to your Node-RED app in order for the flow to work.

<pre class="font-size:15 lang:js decode:true ">[
    {
        "id": "e317bb59.6978a",
        "type": "http in",
        "name": "",
        "url": "/translate",
        "method": "post",
        "x": 216,
        "y": 115,
        "z": "be5832fc.ca9ca8",
        "wires": [
            [
                "e7f0c9c6.0a838"
            ]
        ]
    },
    {
        "id": "e7f0c9c6.0a838",
        "type": "function",
        "name": "",
        "func": "msg.payload = msg.req.body.txt;\nmsg.lang = msg.req.body.lang;\nreturn msg;",
        "outputs": 1,
        "valid": true,
        "x": 454,
        "y": 113,
        "z": "be5832fc.ca9ca8",
        "wires": [
            [
                "8b9210eb.1de0e8"
            ]
        ]
    },
    {
        "id": "8b9210eb.1de0e8",
        "type": "watson-translate",
        "name": "",
        "language": "",
        "x": 665,
        "y": 106,
        "z": "be5832fc.ca9ca8",
        "wires": [
            [
                "1f345415.c4af44"
            ]
        ]
    },
    {
        "id": "1f345415.c4af44",
        "type": "http response",
        "name": "",
        "x": 892,
        "y": 101,
        "z": "be5832fc.ca9ca8",
        "wires": []
    }
]</pre>

&nbsp;