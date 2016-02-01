---
id: 174
title: Open Standards, IBM Social Business Toolkit, and Embedded Experiences
date: 2011-02-04T19:27:49+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.wordpress.com/?p=174
permalink: /2011/02/04/open-standards-ibm-social-business-toolkit-and-embedded-experiences/
jabber_published:
  - 1296847673
  - 1296847673
email_notification:
  - 1296847675
  - 1296847675
categories:
  - Lotus
  - Lotusphere
  - Lotusphere 2011
  - OpenSocial
  - Shindig
  - Vulcan
---
You may have not found this years OGS to be the best OGS ever, however if you are a developer there were some key take aways that should have made your ears perk up.  You probably heard the words &#8220;Open Standards&#8221;, &#8220;IBM Social Business Toolkit&#8221;, and &#8220;Embedded Experiences&#8221; repeated several times during the OGS.  (You also may have heard the words &#8220;native flash&#8221; 20 times in the span of 15 mins&#8230;but thats a different topic for another day.)  If you&#8217;re developer the fact that IBM is focusing on open standards should make you rejoice.  IBM has a good history of using and supporting open standards when it comes to software, and its good to see that tradition continuing as we move forward with all our Lotus products.  The adoption of these open standards is really being pushed by Project Vulcan.  Some of the standards we will be using, and that were mentioned in the OGS were, <a href="http://dev.w3.org/html5/spec/Overview.html" target="_blank">HTML5</a>, <a href="http://docs.opensocial.org/display/OS/Home" target="_blank">OpenSocial</a>, <a href="http://activitystrea.ms/" target="_blank">ActivityStreams</a>, and <a href="http://www.oasis-open.org/committees/tc_home.php?wg_abbrev=cmis" target="_blank">CMIS</a>.

Not only will Lotus be using these standards but driving them as well.  By driving the standards we will be able to contribute the innovations we come up with back to the community.  Also since all of these standards are open that means our customers and business partners will be able to help drive these standards as well by getting involved.  In Connections 3.0 we have already leveraged one of the above open standards, CMIS.  See Luis Benitez&#8217;s <a href="http://www.lbenitez.com/2010/11/download-now-files-connector-for-lotus.html" target="_blank">blog entry</a> on CMIS support in Lotus Connections.

ActivityStreams will also play a large role in all of our &#8220;Next&#8221; releases.  Take the IBM Social Business Toolkit for example.  The IBM Social Business Toolkit is now available on <a href="https://greenhouse.lotus.com/vulcan/shindig/client/login.jsp" target="_blank">Lotus Greenhouse</a> for everyone to explore.  There has also been a <a href="http://www-10.lotus.com/ldd/appdevwiki.nsf/xpViewCategories.xsp?lookupName=IBM%20Social%20Business%20Toolkit" target="_blank">wiki</a> created with a lot of information on the toolkit.  The IBM Social Business Toolkit is what is backing the activity stream which was <a href="http://www-10.lotus.com/ldd/appdevwiki.nsf/dx/Video_Interacting_with_events_in_an_activity_stream" target="_blank">demoed in the OGS</a>.  The reason why it is called the activity stream is because it&#8217;s based on an open standard called <a href="http://activitystrea.ms/" target="_blank">ActivityStreams</a>.  ActivityStreams is a standard that was defined in order to represent a stream of events.  Streams of events is something that has become very popular with social networking sites.  Facebook, Twitter, Google Buzz, and Connections all have these streams of events.  If you standardize this stream it makes it easier to integrate across all of them, in fact, Facebook, Twitter, and Google Buzz all leverage the ActivityStreams spec.  With the IBM Social Business Toolkit, IBM and Lotus, will also now be leveraging that spec for its streams as well.

So how does OpenSocial play into this?  Well OpenSocial references the ActivityStream spec in it&#8217;s sepc as well.  (There is slight confusion here because there is also a notion of something called Activities in OpenSocial 1.1.  This is different from ActivitiesStreams, however, in OpenSocail 2.0 Activities will be deprecated in favor of ActivityStreams.)  In fact, the IBM Social Business Toolkit uses a reference implementation of OpenSocial called <a href="http://shindig.apache.org/" target="_blank">Shindig</a>, in order to interact with the ActivityStream it produces.  Just look at the URL to the toolkit on Greenhouse: https://greenhouse.lotus.com/vulcan/**shindig**/client/login.jsp.  Like I said Shindig is a reference implementation of the OpenSocial spec.  It is an open source Apache project, that anyone is free to use and contribute to.

So back to OpenSocial&#8230;where else will we be leveraging OpenSocial?  Well, we are also working on making the concept of embedded experiences part of the OpenSocial spec as well.  In fact, I have written a <a href="http://docs.opensocial.org/display/OSD/Embedded+Experiences" target="_blank">proposal for embedded experiences</a> which is being considered for OpenSocial 2.0.  This also means the reference implementation of embedded experiences will be part of the next release of Shindig, as long as the proposal is accepted.  It was also announced at LS11 that XPages would be an OpenSocial container, meaning you will be able to render OpenSocial gadgets inside an XPage.

The fact that Lotus has decided to leverage some of these standards is great for everyone from the customer to the developer.  These standards are developed by a community of individuals both from inside and outside IBM.  It also allows everyone to get involved and have a direct impact in some of the cutting edge features that will be in our future release of all Lotus products.