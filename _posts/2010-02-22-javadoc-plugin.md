---
id: 15
title: Javadoc Plugin
date: 2010-02-22T00:56:29+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.wordpress.com/?p=15
permalink: /2010/02/22/javadoc-plugin/
categories:
  - Client Java UI APIs
tags:
  - eclipse
  - Notes
  - Notes Java UI APIs
---
Any software developer will tell you that when using APIs the most important help you can have is documentation.  When talking about Java, the typical type of documentation for APIs is Javadoc.  Often when we produce the jars for our APIs we don&#8217;t build them with the Javadoc included in order to keep the size down.  Typically you would produce two sets of jars, one for production and one for development which has the Javadoc and sometimes the source.  However this model can break down.  For example, with Notes and our Client UI APIs we can&#8217;t distribute a version of Notes with jars that have the Javadoc included.  Therefore our Notes plugin developers have two options.  Use the Javadoc in the Help or use the documentation published online.  The first option is not always possible since most developers don&#8217;t have Notes open when developing plugins.  The second option is therefore the next best option.

However our plugin developers mostly use Eclipse and as we all know Eclipse has this great feature which will give you the Javadoc for the methods and classes you are using as you develope.  But as I said before you would need the plugin to be  build with the Javadoc, which IBM does not do to keep the size of the plugins smaller.  Eclipse has a solution for this problem in the form of an [extension point](http://help.eclipse.org/help33/index.jsp?topic=/org.eclipse.pde.doc.user/reference/extension-points/org_eclipse_pde_core_javadoc.html).  Essentially this allows you to create a plugin containing the Javadoc and then use the extension point to connect the Javadoc with specific plugins.  In theory then one would just have to install this plugin into Notes and then it would be in the target platform, and therefore provide the Javadoc for our Notes developers inside Eclipse without having Notes or a browser open.    We are going to be investigating providing this type of plugin at least for the Client UI APIs but maybe other APIs as well.

[<img class="size-medium wp-image-19 alignnone" title="Client UI APIs Javadoc" src="http://ec2-107-20-24-186.compute-1.amazonaws.com/wordpress/wp-content/uploads/2010/02/sn1.jpg?w=300" alt="Client UI APIs Javadoc" width="300" height="129" srcset="http://ryanjbaxter.com/wp-content/uploads/2010/02/sn1-300x129.jpg 300w, http://ryanjbaxter.com/wp-content/uploads/2010/02/sn1.jpg 523w" sizes="(max-width: 300px) 100vw, 300px" />](http://ec2-107-20-24-186.compute-1.amazonaws.com/wordpress/wp-content/uploads/2010/02/sn1.jpg)

-RJB