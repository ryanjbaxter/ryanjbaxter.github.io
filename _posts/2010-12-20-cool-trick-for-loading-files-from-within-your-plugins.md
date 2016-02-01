---
id: 137
title: Cool Trick For Loading Files From Within Your Plugins
date: 2010-12-20T22:18:49+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.wordpress.com/?p=137
permalink: /2010/12/20/cool-trick-for-loading-files-from-within-your-plugins/
jabber_published:
  - 1292883531
  - 1292883531
email_notification:
  - 1292883532
  - 1292883532
categories:
  - Eclipse
  - Plugin Development
tags:
  - eclipse
  - plugin development
---
Today while working on a plugin I needed to read in a properties file from within my plugin.  I have done this before but could not remember how to do it, so I started Googling.  I eventually came across this <a href="http://www.vogella.de/blog/2010/07/06/reading-resources-from-plugin" target="_blank">page</a>.  I had found the first method on several other pages while searching, but the second method I had never seen before.  As expected it worked , and is actually a really simple and clean way to load resource files, like text, or in my case, properties files from within your plugins.  Just thought I would share it with everyone.