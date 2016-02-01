---
id: 167
title: Ugly Plugins
date: 2011-01-31T20:54:26+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.wordpress.com/?p=167
permalink: /2011/01/31/ugly-plugins/
jabber_published:
  - 1296507270
  - 1296507270
email_notification:
  - 1296507271
  - 1296507271
categories:
  - Eclipse
  - Lotus
  - Lotusphere
  - Lotusphere 2011
  - Notes
  - Plugin Development
---
Last night I met up with Mikkel Heisterberg and Mat Newman at Lotusphere.  I quickly realized I was in for a long night of bashing about what they hate about plugin development for Notes, and Expeditor in general.  (Of course it was all in good fun and they had a lot of great feedback, but it was 2 on 1, not a very fair fight.)  One of the many topics they brought up was the ability for plugins that and user installs into Expeditor to effect the whole client.  For example Mikkel had brought up an instance where he wrote a plugin that used some third party jar which Notes also used.  However the jar in his plugin was a more updated version and when he installed the plugin into Notes it compleley hosed everything.  Their question was why should any third party plugin effect Notes at all?  Well I had no good argument for that, it&#8217;s true, they shouldn&#8217;t.

&nbsp;

We (IBM) run into these types of problems a lot.  To the end user, when a third party extension, plugin or not, has a problem they think it&#8217;s actually Notes that has the problem.  It got me to thinking what the best way is to handle these types of problems.  Obviously we should give the end user some indication of what extension is having the problem.  This will end up saving us and the customer tons of time and effort.  Mikkel, Matt, and I also talked about giving the end user to disable the extension somehow.  That way if the problem is blocking them they can instantly unblock themselves to get their work done.  (This is assuming of course the extension itself if not critical to getting their work done.)  I am sure there is more we can do, but I will be keeping this in mind as we continue to work on future releases of Expeditor/Notes.