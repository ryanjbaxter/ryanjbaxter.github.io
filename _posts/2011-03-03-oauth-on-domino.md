---
id: 204
title: oAuth On Domino
date: 2011-03-03T01:12:18+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=204
permalink: /2011/03/03/oauth-on-domino/
jabber_published:
  - 1299114738
  - 1299114738
email_notification:
  - 1299114739
  - 1299114739
categories:
  - Lotus
---
I have been trying to wrap my head around <a href="http://oauth.net/core/1.0/" target="_blank">oAuth</a> lately and how we leverage it inside our Lotus products.  I have seen some tweets about requesting oAuth on Domino and I assume the Yellowverse wants oAuth on other Lotus servers as well, not just Domino.  I am trying to wrap my head around the use cases.  oAuth is all about delegation.  You can allow an application to access data from a service on your behalf.  If there was an oAuth provider on Domino, than this would allow you to do this with your Domino data.  Which is great, don&#8217;t get me wrong.  The problem is that it will be difficult to build an application that uses oAuth for API delegation, and have that application run against any Domino server, if the Domino server lives on premise.  If the Domino server is in the cloud, for example LotusLive, than oAuth makes perfect sense.  To develop an application  that uses oAuth to access APIs you need to do have two things:

  1. The oAuth endpoints.
  2. An API key and secret.

Now, if you think about a 3rd party application developer building an application that uses APIs from Domino, how would they know what the oAuth endoints are?  (Remember the endpoints will be different for each on premise Domino deployment.)  Secondly how does the application developer register for the API key and secret?  One easy solution to this is to have an application get this information from a configuration file, and have the admin put this information in the configuration file before deploying the application.  In my opinion this kind of defeats the purpose of oAuth.

If you are familiar with gadgets, than you know that <a href="http://code.google.com/apis/gadgets/docs/oauth.html" target="_blank">gadgets can also use oAut</a>h the access APIs from different services.  Say your building a gadget that is accessing data from a Domino server.  (Or any other on premise Lotus server.)  When a gadget uses oAuth is must do the same thing as any other application using oAuth.  The main difference is that you MUST define the oAuth enpoints in the gadget itself and if you don&#8217;t know what those endpoints are going to be before hand, that is a problem.

With all that said, if the main use case for oAuth on premise is to have internal developers develop applications to be used inside the enterprise that access data from these servers than it makes perfect sense.

I think oAuth has its place both on premise and in the cloud but we will have some issues to work out for the on premise use cases.  I would be interested to hear what people&#8217;s use cases are when it comes to oAuth.