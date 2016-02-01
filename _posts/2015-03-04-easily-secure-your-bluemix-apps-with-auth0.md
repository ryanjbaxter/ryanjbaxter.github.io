---
id: 991
title: Easily Secure Your Bluemix Apps With Auth0
date: 2015-03-04T07:01:12+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=991
permalink: /2015/03/04/easily-secure-your-bluemix-apps-with-auth0/
dsq_thread_id:
  - 4536199028
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
---
<div>
  A topic that often comes up when I speak to other developers building apps on Bluemix is authentication.  Many developers are comfortable with securing their apps in traditional development models.  In the cloud, authentication is often slightly different.  For example, you must store session data in an external data store instead of relying on what the app container provides.  When you want to connect to an existing LDAP, Active Directory, or SAML service, that is even more daunting
</div>

<div>
</div>

&nbsp;

<div>
</div>

<div>
  Recently IBM released a <a href="https://developer.ibm.com/bluemix/2015/02/16/easily-secure-access-bluemix-apps-with-sso/" target="_blank">new version of the SSO service</a> in Bluemix which addresses some of these concerns.  You can create a user directory in the cloud to store credentials for authenticated users there and you can setup the service to use an existing SAML, LDAP, or Active Directory service.  You can even enable apps to login with Google, Facebook, and LinkedIn.  However this service only works if your app is using Node.js or Java with Liberty.
</div>

&nbsp;

<div>
  I recently came across a 3rd party service called <a href="https://auth0.com/" target="_blank">Auth0</a>.  I was blown away by the service.  It was very well thought out and covered a number of different use cases, not just for consumer apps but enterprise apps as well.  Like most cloud services for developers you can get a free account to use for developing your apps.  Once you signup you will need to register an application with Auth0.  The process of registering an app and <a href="https://auth0.com/docs" target="_blank">getting started</a> was probably my favorite part of using this service.  You pick the type of app you are building (native mobile, single page web app, regular web app, or hybrid mobile app) and you pick the language you want to use.  They support all the popular ones, Java, Node, Ruby, .Net, Scala, PHP, Python, and more.  Then you presented with details on how to integrate Auth0 into your application including a full working application!
</div>

&nbsp;

<div>
  Like any good developer I downloaded the working app (in my case I picked a regular Java web app) and started it up.  The only gotcha when trying out the sample code is you need to make sure you go to your app’s settings page in the Auth0 dashboard and register a callback URL for localhost.  After doing that the app worked perfectly!  I was really impressed with what you get out of the box.
</div>

&nbsp;

<div>
  Without doing anything, just downloading some code, my sample app had the ability to register users and sign in with Google+.  When a user registers they get an email notification confirming their registration (this email can be customized to your liking from your app settings page).  If you want to allow users to login with other social services in addition to or instead of Google like Facebook, Twitter, etc., it is a matter of going back to your apps settings and clicking the ones you want to use and all of a sudden those services are available.  You don’t have to make any changes to your applications code or go to those services and register your app.  Super easy!
</div>

&nbsp;

<div>
  I was really impressed by all of this, but that was only the beginning.
</div>

&nbsp;

<div>
  Say you have an existing database with users in it you want to use for your app.  No problem, you can <a href="https://auth0.com/docs/mysql-connection-tutorial" target="_blank">configure Auth0 to use that database</a> to lookup and authenticate users.
</div>

<div>
</div>

<div>
  Say you want to use Apples Touch ID to authenticate users in your mobile app, <a href="https://auth0.com/blog/2014/10/27/simplekeychain-a-keychain-library-with-ios-8-and-touchid-support/" target="_blank">they support that too</a>!
</div>

&nbsp;

<div>
  Now to the best features though….
</div>

&nbsp;

<div>
  Say you have an existing LDAP server, or <a href="https://auth0.com/docs/ad" target="_blank">Active Directory server</a>, or <a href="https://auth0.com/docs/saml-configuration" target="_blank">SAML provider</a> you want to use.  They support that too!  These services can even be running on premise behind a firewall.  Like the IBM SSO solution, you need to install a <a href="https://auth0.com/docs/connector" target="_blank">connector</a> that Auth0 uses to communicate with your user directory but doing that is quite simple.  On Windows you run an installer to set it up, on Mac or Linux you run a simple Node.js app.  You can even set the connector up in high availability mode to ease concerns about a single point of failure.
</div>

&nbsp;

<div>
  There are many more features that Auth0 provides, but so far it has covered a wide range of use cases I have come across and does so in a very easy strait forward manner.  I highly encourage you to check Auth0 out.  I have created two videos you can watch below on how easy it is to use this service with your applications running in Bluemix.  The first one shows setting up a basic web app.  The second video then shows how I enabled that web app to authenticate against an LDAP directory running behind a firewall on my local dev machine!
</div>

&nbsp;

<div>
  <span class="youtube"></span>
</div>

&nbsp;

<div>
  <span class="youtube"></span>
</div>