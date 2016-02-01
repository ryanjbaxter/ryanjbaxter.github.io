---
id: 211
title: Defining Default Preferences For Your Plugins
date: 2011-03-15T01:23:35+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=211
permalink: /2011/03/15/defining-default-preferences-for-your-plugins/
jabber_published:
  - 1300152218
  - 1300152218
email_notification:
  - 1300152219
  - 1300152219
categories:
  - Eclipse
  - Lotus
  - Plugin Development
---
Most plugins have some kind of preferences, which show up in the File -> Preferences dialog inside of Notes.  The majority of time, these preferences come pre-configured with some default preference values.  The way to do this is to use an extension point provided by Eclipse called <a href="http://help.eclipse.org/helios/index.jsp?topic=/org.eclipse.platform.doc.isv/reference/extension-points/org_eclipse_core_runtime_preferences.html" target="_blank">org.eclipse.core.runtime.preferences</a>.  You can then provide an initializer class to this extension point that will get called when you first try to access any of the preferences.  Your preference initializer class should subclass org.eclipse.core.runtime.preferences.AbstractPreferenceInitializer.

<pre>&lt;extension point="org.eclipse.core.runtime.preferences"&gt;
         &lt;scope name="foo" class="com.example.FooPrefs"/&gt;
<strong>         &lt;initializer class="com.example.MyPreferenceInitializer"/&gt;</strong>
         &lt;modifier class="com.example.MyModifyListener"/&gt;
 &lt;/extension&gt;</pre>

&nbsp;