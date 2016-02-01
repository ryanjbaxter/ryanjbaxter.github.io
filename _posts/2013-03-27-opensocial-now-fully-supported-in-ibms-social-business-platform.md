---
id: 417
title: 'OpenSocial Now Fully Supported In IBM&#8217;s Social Business Platform'
date: 2013-03-27T16:41:09+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=417
permalink: /2013/03/27/opensocial-now-fully-supported-in-ibms-social-business-platform/
categories:
  - Connections
  - Notes
  - OpenSocial
---
One of the new features announced in the release of Notes and Domino 9.0 Social Edition last week was support for OpenSocial in Notes and iNotes.  This should not come to as a surprise to most of you as we (IBM) have been touting this feature for a while now.  None the less, if you have been living under a rock for the past year or so here is some background on OpenSocial.

At a high level OpenSocial is an open standard developers can use in order to build social applications.  The OpenSocial Foundation was originally founded by Google back in 2007 to combat the Facebook app craze.  While Google never succeeded with killing off Facebook apps ,OpenSocial did not die with that effort.  Many companies which produce enterprise level software started to become interested in OpenSocial because of the gadget model it uses for building apps.  Yes, these are pretty much the same as Google gadgets.  If you are not familiar with the gadget model, think of it as HTML, JavaScript, and CSS wrapped in an XML file.  So anything you can do in a normal web app you can do within a gadget as well.  Besides the app model OpenSocial gadgets run in a very secure manor by using iFrames running on a separate domain from pretty much everything else.  When social business became the hot topic within the enterprise space OpenSocial really became popular because OpenSocial provides numerous APIs for accessing the social data within the application the gadget is running.

So what does all this OpenSocial support buy you in Notes and Domino 9.0?  A few things actually.  If you have Connections 4 in your organization in addition to Notes and Domino 9.0 you get enhanced integration with Connections in your Notes and iNotes clients.  In both Notes and iNotes, the email notifications you get from Connections have the ability to render as embedded experiences.  This means that you can interact with the email, allowing you to comment, share, like, follow, etc., right from the email without having to go to Connections.  In all actuality what you are seeing in your email is actually an OpenSocial gadget.  In addition, in Notes, you can access your Connections activity stream right from within the client and this is done using an OpenSocial gadget as well.  I have been using these features internally for almost a year now and I can tell you they are pretty useful.  I use them almost every day and they do make me more productive, and yes more social.  (I hate using that term but it is true in this case.)  The reason why I say they make me more social is because they encourage me to engage with what is happening in Connections by making it easier to do so.  I will be honest, I don&#8217;t live in Connections, but I don&#8217;t necessarily live in Notes or iNotes either.  I am a developer so I live in my IDE but I always have Notes or iNotes open.  Sometimes I have Connections open but not necessarily.  When I do take a break from coding to go check my email and I have these &#8220;active notification&#8221; emails (ie embedded experiences) from Connections in my inbox I almost always engage with the content within them.  I now also follow what is happening in Connections with the activity stream because at the same time I am checking my email I can browse my activity stream right in my sidebar.  So yeah, I do think it is useful and I am not just saying it because I wrote a lot of the code 😉

The Connections integration is really only half of the story though.  I talked a little bit about the app model above and to me that is really where OpenSocial shines.  Imagine this for a moment.  You are a developer and you have built this awesome app which you have sold to Foo Corp.  Foo Corp asks you if you can integrate the app into their social business suite, which just happens to be Notes, iNotes, and Connections.  Prior to Notes and Domino 9.0 and Connections 4 this was a pretty tough task.  You could build an Eclipse plugin for Notes and you could build an iWidget for Connections, but you didn&#8217;t really have a way to integrate easily into iNotes.  Besides that maybe you have never build an Eclipse plugin before so you know that is going to be a huge learning curve.  Bottom line, this would have been a very costly task.  Now with Connections 4 and Notes and Domino 9 this becomes cheaper and easier.  You can build an OpenSocial gadget to do the integration the customer is looking for and the same gadget will run in Notes, iNotes, and Connections!  No you did not read that wrong, you build the gadget once and it will work in all three products!  Besides that you are just writing HTML, JavaScript, and CSS which you already know so the learning curve is low.  Besides the cool integration story you can also take advantage of embedded experiences in mail and the activity stream.  Overal I think this is a very powerful app dev model and I think it is a huge step forward from where we (IBM) were, especially with regards to Connections and iNotes.

Now with all that said, is OpenSocial perfect? No.  Is the integration of OpenSocial in Notes and Domino and Connections perfect?  Definitely not. Some examples&#8230;.

You can only use OAuth to authorize users in your OpenSocial gadget, there is no support for basic auth, LTPA, or SAML for authentication.  The documentation for building gadgets is lacking, so getting started can be frustrating.  Configuring the OpenSocial component of Notes and Domino can difficult and error prone if you aren&#8217;t careful in following the directions.  Deploying gadgets in Connections requires you to edit files on the server and run a bunch of WAS commands.  So yeah it is not perfect, and we do have gaps and areas where we can improve.  The good news is that we know there is room for improvement.  There is an effort under way right now to write a &#8220;cookbook&#8221; for installing and configuring the OpenSocial Component on Notes and Domino 9.0.  While this won&#8217;t decrease the amount of steps you need to take to configure everything it will present them to you in a fashion that is digestible and explains the why behind the configuration you have to do.  We are also looking at improving the developer story behind building OpenSocial gadgets but I will save that for a future blog post.

Don&#8217;t let the problems I listed above scare you aways from taking advantage of these features.  In my mind any pain you may experience is worth it in the end.  Like I said above, these features are useful and they do make a difference to both end users and developers.  If you get stuck, are frustrated, or generally feel like you want to strangle who ever wrote this stuff, reach out to me.  You can leave a comment on my blog, send me a tweet on Twitter (@ryanjbaxter), find me on Skype (ryanjasonbaxter), or find me on Sametime.  I would be happy to help in any way possible.  However the best way to ask questions is in the forum.  That way the biggest audience possible can help you with your problems and answer your questions.