---
id: 1111
title: 'Building Cloud Native Apps With Spring &#8211; Part 2'
date: 2015-09-21T15:18:37+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1111
permalink: /2015/09/21/building-cloud-native-apps-with-spring-part-2/
dsq_thread_id:
  - 4535002038
categories:
  - Cloud
  - Cloud Foundry
  - PaaS
---
In [part 1](http://ryanjbaxter.com/2015/09/14/building-cloud-native-apps-with-spring-part-1/) of this tutorial, I talked about some of the features of Spring Cloud and we started to build a microservice app that will display a list of obstacle course races.  At the end of part 1 we had three services/apps one which served race data, another which served race participant data, and the last which served our web app.  We ended part 1 when we ran into a cross domain issue when trying to make a request to our races service from our web service.

As I mentioned, there are a number of ways to address the cross domain problem such as using CORS, but that solution does not scale well in a microservices based architecture.  Spring Cloud Netflix has two projects which can help solve this problem in a clean way which supports microservice architectures.  The first project, Eureka, will allow us to setup up service discovery for all the services in our microservices app.  Eureka has both server and client components.  The Eureka server is what all the clients register with and what stores the list of available services and where they are located (their URLs).  The Eureka client component is what we will integrate into each one of our microservices.

The second project, Zuul, integrates with Eureka and allows us to setup a reverse proxy to call our services from our web app.

## Setting Up An Eureka Server

Head over to <a href="http://start.spring.io" target="_blank">start.spring.io</a> and fill out the form following the screen shot below.

[<img class="alignnone size-full wp-image-1112" src="http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-10-at-2.10.52-PM.png" alt="Screen Shot 2015-09-10 at 2.10.52 PM" width="1572" height="1052" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-10-at-2.10.52-PM-300x201.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-10-at-2.10.52-PM-1024x685.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-10-at-2.10.52-PM.png 1572w" sizes="(max-width: 1572px) 100vw, 1572px" />](http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-10-at-2.10.52-PM.png)

Click the Generate button to download the source code and then import the new project into your IDE.  If you open the POM file for this new project you will notice the following dependency

<pre class="font-size:15 lang:default decode:true ">&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
			&lt;artifactId&gt;spring-cloud-starter-eureka-server&lt;/artifactId&gt;
		&lt;/dependency&gt;</pre>

This is our first time seeing a Spring Cloud dependency.  It works much like the other &#8220;starter&#8221; dependcies for Spring Boot.  The versions are managed by the Spring Cloud starter parent, which you will find in the dependency management section of the POM as well.

To make this Spring Boot application act as a Eureka server all we need to do is add a single annotation to our application class and add a couple of properties to our application properties/YAML file.  Open _OcrEurekaApplication.java_ in _com.ryanjbaxter.spring.cloud.ocr.eureka_.  At the top of the class file, either above or below _@SpringBootApplication_ add _@EnableEurekaServer_.  Your class file should look like this.

<pre class="font-size:15 lang:default decode:true">package com.ryanjbaxter.spring.cloud.ocr.eureka;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;

@SpringBootApplication
@EnableEurekaServer
public class OcrEurekaApplication {

    public static void main(String[] args) {
        SpringApplication.run(OcrEurekaApplication.class, args);
    }
}
</pre>

And thats it, no code to write at all!

Now go to _src/main/resources_ and rename _application.properties_ to _application.yml_ and open the file _application.yml_.  Add the following properties to your YAML file

<pre class="lang:default decode:true ">server:
  port: 8761
  
eureka:
  instance:
    hostname: localhost
  client:
    registerWithEureka: false
    fetchRegistry: false
    serviceUrl:
      defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/</pre>

Everything in the eureka section of the YAML file is technically optional but eliminates some unecessary noise in the log files for now.  Eureka out of the box is setup for high availability, so it expects that there be a second Eureka server is can replicate information with.  Since we are just running locally for now, we don&#8217;t need to worry about this, so these properties are just disabling that replication.  If you didn&#8217;t add these properties everything would still work fine, but you would see some errors in the logs.

To start our Eureka server just run the Spring Boot app.  Once the app starts go to <a href="http://localhost:8761" target="_blank">http://localhost:8761</a> and you should see a nice Eureka dashboard that will list all the services that are registered with Eureka.  Currently there are none so lets fix that.

## Enabling Eureka Clients

Lets get our services configured to be Eureka clients.  First we need to name our services.  By default Spring Cloud Netflix will use the application name as the service name when registering services with Eureka.  To give our application&#8217;s names we can set a specific Spring Boot property.  In the _src/main/resources_ folder of the races, participants, and web services create a file called _bootstrap.yml_.  Within the _bootstrap.yml_ files add the following properties.

<pre class="font-size:15 lang:default decode:true ">spring:
  application:
    name: web</pre>

The above code snippet is an example _bootstrap.yml_ file from the web service (we are giving the app the name web).  In the bootstrap.yml files for the races and participants services change the _name_ property to _races_ and _participants_ respectively.

Now that our services have names lets add the Eureka client dependencies to them.  In the POM files for the races, participants, and web services add the following dependency management section

<pre class="font-size:15 lang:default decode:true ">&lt;dependencyManagement&gt;
		&lt;dependencies&gt;
			&lt;dependency&gt;
				&lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
				&lt;artifactId&gt;spring-cloud-starter-parent&lt;/artifactId&gt;
				&lt;version&gt;Angel.SR3&lt;/version&gt;
				&lt;type&gt;pom&lt;/type&gt;
				&lt;scope&gt;import&lt;/scope&gt;
			&lt;/dependency&gt;
		&lt;/dependencies&gt;
	&lt;/dependencyManagement&gt;</pre>

In addition you will want to add the following dependency to all three POMs

<pre class="font-size:15 lang:default decode:true ">&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
			&lt;artifactId&gt;spring-cloud-starter-eureka&lt;/artifactId&gt;
		&lt;/dependency&gt;</pre>

That takes care of our dependencies, now we can make each service a client by adding a single annotation to the application class file and adding some properties to the application&#8217;s properties file.  Open the application class file for each service and add @EnableDiscoveryClient to the class file.  For example, here is what the application class file for the web service

<pre class="font-size:15 lang:default decode:true">package com.ryanjbaxter.spring.cloud.ocr.web;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient; 

@SpringBootApplication 
@EnableDiscoveryClient
public class OcrWebApplication {

    public static void main(String[] args) {
        SpringApplication.run(OcrWebApplication.class, args);
    }
}
</pre>

Now open the application.yml file for each service and add the following properties

<pre class="font-size:15 lang:default decode:true ">eureka:
  client:
    serviceUrl:
      defaultZone: http://localhost:8761/eureka/</pre>

These properties just tell the Eureka client where the Eureka server is located so it knows where to register itself.

Start all the apps, the Eureka server, the races service, the participants service, and the web service.  It takes a few minutes for all the services to register themselves with Eureka.  If you watch the logs you should see an indication that registrations are taking place.  For example here are the logs from the races service when it registers itself with Eureka.

<pre class="font-size:15 lang:default decode:true ">2015-09-10 10:21:56.762  INFO 5149 --- [pool-3-thread-1] com.netflix.discovery.DiscoveryClient    : Disable delta property : false
2015-09-10 10:21:56.763  INFO 5149 --- [pool-3-thread-1] com.netflix.discovery.DiscoveryClient    : Single vip registry refresh property : null
2015-09-10 10:21:56.763  INFO 5149 --- [pool-3-thread-1] com.netflix.discovery.DiscoveryClient    : Force full registry fetch : false
2015-09-10 10:21:56.763  INFO 5149 --- [pool-3-thread-1] com.netflix.discovery.DiscoveryClient    : Application is null : false
2015-09-10 10:21:56.763  INFO 5149 --- [pool-3-thread-1] com.netflix.discovery.DiscoveryClient    : Registered Applications size is zero : true
2015-09-10 10:21:56.763  INFO 5149 --- [pool-3-thread-1] com.netflix.discovery.DiscoveryClient    : Application version is -1: false
2015-09-10 10:21:56.768  INFO 5149 --- [pool-3-thread-1] com.netflix.discovery.DiscoveryClient    : Getting all instance registry info from the eureka server
2015-09-10 10:21:56.769  INFO 5149 --- [pool-3-thread-1] com.netflix.discovery.DiscoveryClient    : The response status is 200
2015-09-10 10:21:56.851  INFO 5149 --- [pool-2-thread-1] com.netflix.discovery.DiscoveryClient    : DiscoveryClient_RACES/ryans-macbook-pro.local - Re-registering apps/RACES
2015-09-10 10:21:56.851  INFO 5149 --- [pool-2-thread-1] com.netflix.discovery.DiscoveryClient    : DiscoveryClient_RACES/ryans-macbook-pro.local: registering service...
2015-09-10 10:21:56.885  INFO 5149 --- [pool-2-thread-1] com.netflix.discovery.DiscoveryClient    : DiscoveryClient_RACES/ryans-macbook-pro.local - registration status: 204
2015-09-10 10:22:06.770  INFO 5149 --- [scoveryClient-2] com.netflix.discovery.DiscoveryClient    : DiscoveryClient_RACES/ryans-macbook-pro.local - retransmit instance info with status UP
2015-09-10 10:22:06.770  INFO 5149 --- [scoveryClient-2] com.netflix.discovery.DiscoveryClient    : DiscoveryClient_RACES/ryans-macbook-pro.local: registering service...
2015-09-10 10:22:06.779  INFO 5149 --- [scoveryClient-2] com.netflix.discovery.DiscoveryClient    : DiscoveryClient_RACES/ryans-macbook-pro.local - registration status: 204</pre>

If you see this in the logs of your services than you know things are working.  Once you start seeing these logs you can go to <a href="http://localhost:8671" target="_blank">http://localhost:8671</a> and check the Eureka dashboard.  You should see something similar to the screenshot below with all the services registered.

[<img class="alignnone size-full wp-image-1113" src="http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-10-at-2.44.18-PM.png" alt="Screen Shot 2015-09-10 at 2.44.18 PM" width="1355" height="209" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-10-at-2.44.18-PM-300x46.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-10-at-2.44.18-PM-1024x158.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-10-at-2.44.18-PM.png 1355w" sizes="(max-width: 1355px) 100vw, 1355px" />](http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-10-at-2.44.18-PM.png)

## Setting Up A Reverse Proxy With Zuul

Zuul will use the Eureka server in order to know how and where to route incoming requests.  We will integrate Zuul into our web service so our client side code can make requests back to the server on the same domain and avoid any cross domain issues.

First we need to add the Zuul starter dependency to the POM of our web service.  Open the POM file and add the following dependency.

<pre class="lang:default decode:true ">&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
			&lt;artifactId&gt;spring-cloud-starter-zuul&lt;/artifactId&gt;
		&lt;/dependency&gt;</pre>

Again turning on Zuul in our application is as simple as adding another annotation to our application class file and adding some properties to our application&#8217;s properties file.  Open _OcrWebApplication.java_ in _com.ryanjbaxter.spring.cloud.ocr.web_ and add _@EnableZuulProxy_ to the class file.

<pre class="font-size:15 lang:default decode:true">package com.ryanjbaxter.spring.cloud.ocr.web;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.netflix.zuul.EnableZuulProxy;

@SpringBootApplication
@EnableZuulProxy
@EnableDiscoveryClient
public class OcrWebApplication {

    public static void main(String[] args) {
        SpringApplication.run(OcrWebApplication.class, args);
    }
}</pre>

Now open _application.yml_ for the web service and add the following properties

<pre class="lang:default decode:true ">zuul:
  routes:
    races: /races/**
    participants: /paticipants/**</pre>

By default Zuul will forward requests to path xyz to service xzy.  For example if you were to make a request to http://localhost:8080/races it would forward that request to the races service and call http://localhost:8282/.  However if you were to make a request to http://localhost:8080/races/123 it would not know what to do with it because it doesn&#8217;t know about the additional path information.  In the properties above we are just telling Zuul to forward all requests to /races/\*\* to the races service and all requests to /participants/\*\* to the participants service.

After making the above changes to the web service restart the application.  Once it has reregistered with Eureka trying using the reverse proxy to proxy requests to the services.  For example, you should be able to open a browser and go to <a href="http://localhost:8080/races" target="_blank">http://localhost:8080/races</a> and get back the array of race information.  It should look exactly the same as if you went to the service directly by going to <a href="http://localhost:8282" target="_blank">http://localhost:8282</a>.  Similarly you should be able to go to <a href="http://localhost:8080/participants" target="_blank">http://localhost:8080/participants</a> and get back the array of participants.  If those work than everything is setup correctly and you are good to go.  We can now use the reverse proxy and Eureka to finish implementing our web app.