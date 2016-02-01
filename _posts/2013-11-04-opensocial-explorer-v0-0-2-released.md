---
id: 455
title: OpenSocial Explorer v0.0.2 Released!
date: 2013-11-04T21:10:15+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=455
permalink: /2013/11/04/opensocial-explorer-v0-0-2-released/
categories:
  - OpenSocial
---
Today I released version 0.0.2 of the OpenSocial Explorer.  From an end user perspective not a lot has changed, although there are a couple of differences.

  1. The navigator on the left has been updated to use the Dojo Tree control to make it easier to navigate the different sample gadgets.
  2. There is now a Login link in the header that allows you to log in with Facebook and Google OAuth and OpenID.  This is not really used for anything yet but hopefully in later releases we can use this to configure gadgets on a per user basis <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />
  3. There is now a button in the navigator that allows you to create new gadgets.  This gadgets you create will not be persisted, yet, but allows you to start fresh with your sample gadgets.

Under the covers there has been a lot of changes.  In the 0.0.1 release it was nearly impossible to reuse the individual Dijits because they all were so dependent on each other.  Now each Dijit can be used independent of each other.  We have added <a href="http://opensocial.github.io/explorer/opensocial-explorer-webcontent/jsdoc/index.html" target="_blank">JavaScript documentation</a> for all the modules and as with last release there is JavaDoc as well.

The war file for the OpenSocial Explorer can be downloaded from the <a href="http://opensocial.github.io/explorer/download.html" target="_blank">project site</a>.  In addition you will be able to find the source and individual jar files for the different projects.  If you are using Maven you can add dependencies on the OpenSocial Explorer.