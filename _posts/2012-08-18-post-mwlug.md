---
id: 367
title: Post MWLUG
date: 2012-08-18T11:59:13+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=367
permalink: /2012/08/18/post-mwlug/
categories:
  - Lotus
  - OpenSocial
  - Shindig
  - Web Development
---
I had a great time at MWLUG.  LUGs are a different beast than Lotusphere, because they are so much smaller there is much more interaction which is great!  I also got to attend other sessions, which was personally really enlightening. Listening to consumers of the (mostly) great products we (IBM) produce give me as an IBMer a completely different perspective on things.  Personally I learned that maybe my sessions are a little to technical for most people  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> This is why XPages is soooo successful, you don&#8217;t need a degree in computer science in order to build an excellent app using XPages.  However if you do have that type of background you can really do some pretty cool stuff, which some of the speakers illustrated in their sessions.  However we (IBM) need to make the barrier to entry for our app developers low, that way we get the most adoption.  I plan on keeping this in mind, and working on refining our message on OpenSocial as we get closer to Lotusphere.  We need to start working, maybe even in a hands on way, to get feedback from our existing app developers on how they want to use all the cool stuff OpenSocial provides, but to do that we need to make it easy for them to use it first.

If you are interested in taking a look at my slides from MWLUG I have them hosted at <a href="http://mwlugpres.ryanjbaxter.com" target="_blank">http://mwlugpres.ryanjbaxter.com</a>.  (The OAuth demo will not work only because I disabled it because it was a little insecure the way I hosted it.)  I am not sure how long I will keep them up there only because I also created a Github project for them.  I guess you are wondering why I would do that?  Well it is because they aren&#8217;t your normal slides, I build them using OpenSocial and an HTML5 Javascript library called <a href="http://lab.hakim.se/reveal-js/" target="_blank">reveal.js</a>.  The project depends on Apache Shindig and provides the source code for the gadget as well as the container to run the gadget in.  I wouldn&#8217;t focus too much on the container because you don&#8217;t really need to know about that, the gadget should be your primary focus.  The project uses Maven so you need to install Maven and then build the code.  This will produce a war file that you can then run on any J2EE container, such as Tomcat or Jetty.  You can find the project on <a href="https://github.com/ryanjbaxter/OpenSocial-Presentation" target="_blank">Github</a>.  If you need help using it let me know.