---
id: 85
title: 'Part 3: Creating Your First Lotus Notes Plugin'
date: 2010-11-29T21:44:38+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.wordpress.com/?p=85
permalink: /2010/11/29/part-3-creating-your-first-lotus-notes-plugin/
jabber_published:
  - 1291067083
  - 1291067083
email_notification:
  - 1291067085
  - 1291067085
categories:
  - Client Java UI APIs
  - Lotus
  - Notes
  - Tutorial
  - VideoFest
tags:
  - eclipse
  - Expeditor
  - Java
  - Notes
  - VideoFest
---
In part three of this tutorial we will be leveraging some APIs provided by Lotus Notes to change the behavior of our context menu.  We also take a quick look at how to debug your plugins for the Eclipse IDE.

<span class="youtube"></span>

If you are interested in learning more about the Notes Client Java UI APIs (com.ibm.notes.java.ui plugin from the video) please check out the API documentation on the [wiki](http://www-10.lotus.com/ldd/ddwiki.nsf/xpViewCategories.xsp?lookupName=APIs%3A%20Notes%20Client%20Java%20UI "Notes and Domino App Dev Wiki").  The Javadoc for the APIs are on the wiki, <a title="8.5.1 Javadoc" href="http://www-10.lotus.com/ldd/ddwiki.nsf/dx/Notes_Client_Java_UI_APIs-v8.5.1" target="_blank">8.5.1</a> and <a title="8.5.2 Javadoc" href="http://www-10.lotus.com/ldd/ddwiki.nsf/dx/Notes_Client_Java_UI_APIs-v8.5.2" target="_blank">8.5.2</a>.

I also left the enablement/disablement of the context menu up to you.  You can find more information the org.eclipse.ui.menus extension point and the visiblewhen and enablement child elements <a title="org.eclipse.ui.menus" href="http://help.eclipse.org/ganymede/index.jsp?topic=/org.eclipse.platform.doc.isv/reference/extension-points/org_eclipse_ui_menus.html" target="_blank">here</a>.  You will need to make sure the current element adapts to either a NotesUIDocument or NotesUIView.  Enablement can be tested in the visiblewhen and enablement child elements with the adapt element.

Lastley here is the updated code for our adapter class.

package com.acme.demo.helloworld;

import org.eclipse.core.commands.AbstractHandler;
  
import org.eclipse.core.commands.ExecutionEvent;
  
import org.eclipse.core.commands.ExecutionException;
  
import org.eclipse.swt.widgets.MessageBox;
  
import org.eclipse.ui.PlatformUI;

import com.ibm.notes.java.ui.NotesUIElement;
  
import com.ibm.notes.java.ui.NotesUIWorkspace;

public class HelloWorldHandler extends AbstractHandler {

private static final String MESSAGE = &#8220;Hello World &#8220;;

public Object execute(ExecutionEvent event) throws ExecutionException {
  
NotesUIWorkspace ws = new NotesUIWorkspace();
  
NotesUIElement element = ws.getCurrentElement();
  
if(element != null){
  
MessageBox messageBox = new MessageBox(PlatformUI.getWorkbench().getActiveWorkbenchWindow().getShell());
  
messageBox.setMessage(&#8220;The title of the current element is: &#8221; + element.getTitle());
  
messageBox.open();
  
}

return null;
  
}

}