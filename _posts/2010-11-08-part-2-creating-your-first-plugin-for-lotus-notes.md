---
id: 62
title: 'Part 2: Creating Your First Plugin For Lotus Notes'
date: 2010-11-08T14:24:29+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.wordpress.com/?p=62
permalink: /2010/11/08/part-2-creating-your-first-plugin-for-lotus-notes/
jabber_published:
  - 1289226276
  - 1289226276
categories:
  - Lotus
  - Notes
tags:
  - eclipse
  - Expeditor
  - Java
  - Lotus
  - Notes
---
In the second part of this tutorial, we will use what we learned in part one, and create a new client services (plugin) project which will contribute a context menu to Lotus Notes.  The video explains everything your need to do, but if your would like more information on some of the topics covered in this video check out the links below.

(For the best quality watch the video in 720P.)

<span class="youtube"></span>

**Architecture**

This is a high level architecture diagram of the RCP layer inside of Lotus Notes.  The big red box in the diagram is RCP or rich client platform.  This platform contains several libraries which provide both extension points and APIs you can use in your plugins.

[<img class="size-medium wp-image-63 alignnone" title="design" src="http://ec2-107-20-24-186.compute-1.amazonaws.com/wordpress/wp-content/uploads/2010/11/design.jpg?w=300" alt="" width="300" height="196" />](http://ec2-107-20-24-186.compute-1.amazonaws.com/wordpress/wp-content/uploads/2010/11/design.jpg)

**Documentation**

<a href="http://help.eclipse.org/ganymede/index.jsp?topic=/org.eclipse.platform.doc.isv/reference/extension-points/org_eclipse_ui_menus.html" target="_blank">org.eclipse.ui.menus</a>

<a href="http://help.eclipse.org/ganymede/index.jsp?topic=/org.eclipse.platform.doc.isv/reference/extension-points/org_eclipse_ui_commands.html" target="_blank">org.eclipse.ui.commands</a>

<a href="http://help.eclipse.org/ganymede/index.jsp?topic=/org.eclipse.platform.doc.isv/reference/extension-points/org_eclipse_ui_handlers.html" target="_blank">org.eclipse.ui.handlers</a>

**Code For Handler**

import org.eclipse.core.commands.AbstractHandler;
  
import org.eclipse.core.commands.ExecutionEvent;
  
import org.eclipse.core.commands.ExecutionException;
  
import org.eclipse.swt.widgets.MessageBox;
  
import org.eclipse.ui.PlatformUI;

public class HelloWorldHandler extends AbstractHandler {

private static final String MESSAGE = &#8220;Hello World &#8220;;

public Object execute(ExecutionEvent event) throws ExecutionException {
  
MessageBox messageBox = new MessageBox(PlatformUI.getWorkbench().getActiveWorkbenchWindow().getShell());
  
messageBox.setMessage(MESSAGE);
  
messageBox.open();

return null;
  
}

}