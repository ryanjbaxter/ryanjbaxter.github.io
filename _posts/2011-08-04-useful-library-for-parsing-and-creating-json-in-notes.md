---
id: 257
title: Useful Library For Parsing And Creating JSON In Notes
date: 2011-08-04T01:51:26+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=257
permalink: /2011/08/04/useful-library-for-parsing-and-creating-json-in-notes/
jabber_published:
  - 1312422686
  - 1312422686
email_notification:
  - 1312422686
  - 1312422686
categories:
  - Eclipse
  - Lotus
  - Notes
  - Plugin Development
---
If you have ever tried to work with JSON in an Eclipse plugin you know it is not strait forward.  One of the easiest ways to work with JSON is to use the libraries from <a href="http://www.json.org/java/index.html" target="_blank">json.org</a>.  However if your working with JSON inside of Notes 8.5.2 or later, there is actually a library built into Notes you can use.  Your plugin will need to have a dependency on com.ibm.commons.  In this plugin there are several classes you can use to work with JSON.  The Javadoc for the APIs can be found <a href="http://public.dhe.ibm.com/software/dw/lotus/Domino-Designer/JavaDocs/XPagesExtAPI/8.5.2/com/ibm/commons/util/io/json/package-summary.html" target="_blank">here.</a>  Here is a <a href="https://gist.github.com/554a2fd80015b3f199a1" target="_blank">sample class</a> which will parse a string of JSON into a Java object which you can in turn use to get values from.  As JSON becomes more and more popular with web services, having libraries to deal with JSON objects when your not using Javascript is critical.