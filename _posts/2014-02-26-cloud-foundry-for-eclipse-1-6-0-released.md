---
id: 532
title: Cloud Foundry for Eclipse 1.6.0 Released
date: 2014-02-26T09:11:25+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=532
permalink: /2014/02/26/cloud-foundry-for-eclipse-1-6-0-released/
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
---
Yesterday there was a new <a href="https://groups.google.com/a/cloudfoundry.org/forum/#!topic/vcap-dev/4xelFAvw8ww" target="_blank">release</a> of the <a href="https://github.com/cloudfoundry/eclipse-integration-cloudfoundry" target="_blank">Cloud Foundry plugin</a> for Eclipse. The plugin allows developers using Eclipse to push their apps to a Cloud Foundry instance from within their Eclipse dev environment.  This includes BlueMix as well. It integrates nicely into the Servers view of your Eclipse dev environment and behaves just like a Tomcat server instance would.  Below is a summery of what is in the new release.  You can install the new version from the <a href="http://marketplace.eclipse.org/content/cloud-foundry-integration-eclipse#.Uw31KvRdWPs" target="_blank">Eclipse Marketplace</a> or by installing the <a href="http://dist.springsource.com/release/TOOLS/cloudfoundry" target="_blank">update site</a>.

&nbsp;

> Some of the changes in 1.6.0 include:
> 
> 1. Spring Boot support:
  
> &#8211; In the Project Explorer, right-click on your Spring Boot project, and select Configure -> Enable as Cloud Foundry App.
  
> &#8211; Then simply drag and drop the Spring Boot app to your Cloud Foundry server instance.
> 
> 2. Servers using self-signed certificates:
  
> &#8211; Many private cloud deployments rely on self-signed certificates. Starting this release, we support such configurations.
  
> &#8211; When creating a new Cloud Foundry server through the Eclipse Servers wizard, if self-signed certificate is detected for a cloud URL, the user will be prompted whether to proceed. If so,
  
> the decision will be persisted as a preference for that URL for any future server instance creation and interactions using that URL, including cloning a server instance to another org/space.
> 
> 3. Application environment variables:
  
> &#8211; Users can now specify environment variables both in the application deployment wizard, or edit them after an application is deployed through the Cloud Foundry server editor.
> 
> 4. Free-form application memory choices:
  
> &#8211; We&#8217;ve removed the restriction on application memory choices. Now users can specify any memory choice for applications in MB. For example: 359, 1230, etc..
> 
> 5. Console enhancements:
  
> &#8211; The Cloud Foundry console now displays progress when pushing, starting or stopping an application. The application log files can now be more easily streamed to the console by right-clicking on the application in the Servers view, and selecting &#8220;Show Console&#8221;.