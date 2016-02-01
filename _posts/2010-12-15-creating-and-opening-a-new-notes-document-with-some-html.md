---
id: 130
title: Creating And Opening A New Notes Document With Some HTML
date: 2010-12-15T14:00:13+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.wordpress.com/?p=130
permalink: /2010/12/15/creating-and-opening-a-new-notes-document-with-some-html/
jabber_published:
  - 1292421615
  - 1292421615
email_notification:
  - 1292421616
  - 1292421616
categories:
  - Uncategorized
---
Over the past few months I have gotten several questions about how to create a email from within a plugin that has some HTML in the body and opens that email in the Notes UI.  Essentially they want to do something similar to clicking the new email action button and Notes opening a new memo form with an HTML signature in the body already.  People usually get stuck at two spots.  First they go down the path of using the <a href="http://public.dhe.ibm.com/software/dw/lotus/Domino-Designer/JavaDocs/NotesClientJavaUIAPIs/v852/com/ibm/notes/java/ui/NotesUIWorkspace.html#composeDocument(com.ibm.notes.java.api.data.NotesFormData)" target="_blank">NotesUIWorkspace.composeDocument(NotesFormData)</a> API.  This is a good place to start, but you will quickly find out that using this API will not allow you to add HTML to the document.  Whatever HTML you do add will just show up in the body of the document.  The next place people look is to Notes.jar, the backend Java classes.  They generally will create a new document, add a rich text field and then add a rich text item to that field.  They usually try to add a rich text style to that rich text item with the property set to allow passthru HTML.  Finally they would add the HTML to the rich text item.  Then they save the document and open it using <a href="http://public.dhe.ibm.com/software/dw/lotus/Domino-Designer/JavaDocs/NotesClientJavaUIAPIs/v852/com/ibm/notes/java/ui/NotesUIWorkspace.html#openDocument(boolean, com.ibm.notes.java.api.data.NotesDocumentData)" target="_blank">NotesUIWorkspace.openDocument(boolean, Document)</a>.  However this has a similar result, in that the HTML that you entered shows up in the body of the document.  The <a href="https://gist.github.com/741908" target="_blank">solution</a> is actually a combination of the above two techniques.

The code snippet in the solution comes from a handler, which is used to handle commands in Eclipse.  The code to create the document does not actually need to be in a handler, it can be anywhere.  Let me just walk through the code a little bit.  One thing to take away from this code is that when using the backend Java classes (Notes.jar) inside an Eclipse plugin, you should be putting that code in a NotesSessionJob class.  This class is part of the com.ibm.notes.java.api plugin, so make sure you depend on it in your plugin.  Now to the meat of the code.  It is key that we make sure we don&#8217;t convert MIME to RT, we do this at line 45.  Notice that we are creating a temporary document.  The temporary document is created in a temporary DB which Notes creates on your local file system.  We then set the form and add the necessary fields to the document.  Instead of creating a rich text item for the body we create a MIMEEntry.  We then add the HTML we want to put in the document to the MIMEEntry.  Finally we save the document.  The final key step is to call <a href="http://public.dhe.ibm.com/software/dw/lotus/Domino-Designer/JavaDocs/NotesClientJavaUIAPIs/v852/com/ibm/notes/java/ui/NotesUIWorkspace.html#composeDocument(com.ibm.notes.java.api.data.NotesDatabaseData, lotus.domino.Document)" target="_blank">NotesUIWorkspace.createDocument(NotesDatabaseData, Document)</a>.  This method takes the document, in this case our temporary document, and moves it to the database specified by the NotesDatabaseData object, which in this case it the users mail DB, and then opens that document in the UI.

Hope people find this useful