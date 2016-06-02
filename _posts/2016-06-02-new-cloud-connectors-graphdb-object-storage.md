---
published: true
layout: post
title: "Bluemix Cloud Connectors For IBM Graph DB and Object Storage Services"
categories:
  - Cloud
  - Bluemix
  - Spring
---

As I have blogged about in the [past](http://ryanjbaxter.com/2015/03/02/bluemix-java-developers-your-life-just-got-a-little-easier/), the Bluemix Cloud Connectors project is
a Java library you can use to easily access service credentials and obtain high-level
Java objects to work with various services in Bluemix.  Today I updated the project
to add support for two new services in Bluemix, the [IBM Graph DB service](https://new-console.ng.bluemix.net/catalog/services/ibm-graph/), and
the [Object Storage service](https://new-console.ng.bluemix.net/catalog/services/object-storage/).

Right now, only the snapshot builds contain support for these services.  I will
be pushing a build to the central Maven repository once I am confident the new
code is stable.  See the [README](https://github.com/IBM-Bluemix/bluemix-cloud-connectors/blob/master/README.md#snapshot-builds) in the Bluemix Cloud Connectors project for information
on where to find the snapshot builds.

There are also a couple of differences in the way these services work compared to
some of the other services in the project.  First, the IBM Graph DB service does
not have a Java SDK so the Bluemix Cloud Connectors project cannot provide you with
a high level Java object to work with in your code.  You can use the cloud connectors
project to [get the service credentials](https://github.com/IBM-Bluemix/bluemix-cloud-connectors#accessing-the-service-credentials-in-a-non-spring-app) for the service so you can then make the
necessary REST calls from your app.  Secondly, the Object Storage service requires
additional information besides the normal username, password, and API URL typical
services use.  When using the Object Storage service you also need the domain name
and project for your service instance.  This does not pose much of an issue when running
your app in Bluemix, however there is a "gotcha" when it comes to running your app
locally.  In order to provide this information in a URL format for the local config, the
Object Storage support expects it to be in the path parameters of the URL.  For more
information about supporting these two new services please see the [README](https://github.com/IBM-Bluemix/bluemix-cloud-connectors/blob/master/README.md#supported-services).

As always feedback is welcome, feel free to leave comments below, or create
[issues](https://github.com/IBM-Bluemix/bluemix-cloud-connectors/issues) in the project on GitHub.
