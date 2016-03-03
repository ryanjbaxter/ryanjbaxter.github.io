---
published: false
layout: post
title: O'Reilly Fluent And A New Sensor Tag Demo
---

Next week I will be heading to [O'Reilly Fluent](http://conferences.oreilly.com/fluent/javascript-html-us) out in San Francisco where I will be giving a session [on MQTT and IoT](http://conferences.oreilly.com/fluent/javascript-html-us/public/schedule/detail/46230) as well as manning the IBM booth in the expo hall.  Given the session is not a sponsored session the content presented in the session will be mostly vendor agnostic, but obviously all the concepts presented will apply directly to how you use MQTT as part of the [IBM Internet of Things Foundation](https://internetofthings.ibmcloud.com/).

Since I only have 30 minutes total for the session I wanted to build a demo that uses MQTT that was quick and easy.  I turned to my favorite IoT device, the [TI Sensor Tag](http://www.ti.com/ww/en/wireless_connectivity/sensortag2015/?INTC=SensorTag&HQS=sensortag).  I love the Sensor Tag because it is cheap, only $30, and it packs so many sensors into a tiny package.  TI recently released a new version of the Sensor Tag which has even more sensors, so if you don't have the latest version I suggest you check it out, again its only $30.  In addition to the new version of the Sensor Tag there is also a new version of the iOS and Android application which has some nice integration with the IBM IoT Foundation out of the box!  You can read more about the new app on [developerWorks](https://developer.ibm.com/recipes/tutorials/connect-a-cc2650-sensortag-to-the-iot-foundations-quickstart/).

For my demo at Fluent I decided to take advantage of one of the new sensors on the Sensor Tag, the luxometer.  A luxometer is a sensor which measures the amount of light exposed to the sensor.  I wanted to visualize the luxometer data in a creative way and I eventually came across [this blog post](http://laserred.co/2013/07/html5-lux-meter-tutorial/) which showed how to build and HTML 5 luxometer in the browser.  Adapting the code provided in the blog post I was able to come up with a demo that used the HTML 5 luxometer to display in real time the data coming from the luxometer on the Sensor Tag using the power of MQTT!  Below is a short video on how it all works.  If you happen to be at Fluent please stop by my session or the IBM booth to check out the demo yourself!

<iframe width="420" height="315" src="https://www.youtube.com/embed/7YiL3K9zGfE" frameborder="0" allowfullscreen></iframe>
