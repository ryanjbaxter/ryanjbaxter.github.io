---
id: 965
title: Deploying Spring Boot Apps To Bluemix
date: 2015-02-02T13:36:20+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=965
permalink: /2015/02/02/deploying-spring-boot-apps-to-bluemix-with-spring-cloud-connectors/
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
---
In my previous three posts ([Building &#8220;Bootiful&#8221; Java Apps](http://ryanjbaxter.com/2014/12/08/building-bootiful-java-apps/ "Building “Bootiful” Java Apps"), [Building REST APIs With Spring Boot](http://ryanjbaxter.com/2014/12/17/building-rest-apis-with-spring-boot/ "Building REST APIs With Spring Boot"), [Securing REST APIs With Spring Boot](http://ryanjbaxter.com/2015/01/06/securing-rest-apis-with-spring-boot/ "Securing REST APIs With Spring Boot"))  I have described how to build and secure a Spring Boot app.  In the final post in this series we will deploy the app to <a href="http://bluemix.net" target="_blank">IBM Bluemix</a>.  If you don&#8217;t already have a Bluemix account you can register for a free 30 day trial.  Spring Boot apps can be packaged as either a war or a jar, however there is a small benefit to deploying your application as a war when deploying to Bluemix.  Since the jar will contain Tomcat it has some extra size.  When you package your application as a war the Tomcat dependencies are removed so the package is smaller and there is less to push over the wire.

In addition we will also need to change how we tell our application about the location of the Mongo DB.  Right now the application will assume the Mongo DB is running on localhost, that will not be the case in Bluemix.  Lets address this problem first.

As you probably already know if you are familiar with services in a Cloud Foundry based PaaS, a service&#8217;s credentials are made available to an application via the <a href="https://www.ng.bluemix.net/docs/#cli/index.html#cfcommands" target="_blank">environment variable VCAP_SERVICES</a>.  One option to access the Mongo DB credentials is to parse the VCAP\_SERVICES environment variable extract the credentials for the Mongo DB service and use those to instantiate our Mongo code in our app.  Most applications will also have the requirement that the app be able to run locally, so in addition to parsing the VCAP\_SERVICES environment variable we will also need to add some code to figure out if we are running in the cloud or not.  This code would not be hard to write, but there are better options.  There is a nice library that can do all this for us called <a href="http://cloud.spring.io/spring-cloud-connectors/" target="_blank">Spring Cloud Connectors</a>.  The Spring Cloud Connectors project makes it easy to use client libraries for various services when your application is running in the cloud.  In addition to supporting various cloud environments, the Spring Cloud Connectors project can support the same code running locally as well.  To get started using the Spring Cloud Connectors project we need to add some dependencies to our POM.  Add the following dependencies to your POM file.

&nbsp;

<pre class="font-size:15 lang:default decode:true">&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
			&lt;artifactId&gt;spring-cloud-spring-service-connector&lt;/artifactId&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
			&lt;artifactId&gt;spring-cloud-localconfig-connector&lt;/artifactId&gt;
		&lt;/dependency&gt;
		&lt;dependency&gt;
			&lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
			&lt;artifactId&gt;spring-cloud-cloudfoundry-connector&lt;/artifactId&gt;
		&lt;/dependency&gt;</pre>

The spring-cloud-spring-service-connector artifact provides us the with the necessary magic when running in a Spring app.  The spring-cloud-local-config-connector artifact provides the ability to run the app locally.  The spring-cloud-cloudfoundry-connector project provides the necessary magic when running on PaaS based on Cloud Foundry, like Bluemix.

Now that we have our dependencies, lets add some code.  In the demo package of our application add the following class

<pre class="font-size:15 lang:default decode:true">package demo;

import org.springframework.cloud.config.java.AbstractCloudConfig;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.mongodb.MongoDbFactory;

@Configuration
public class CloudConfig extends AbstractCloudConfig {
  @Bean
  public MongoDbFactory documentMongoDbFactory() {
      return connectionFactory().mongoDbFactory();
  }
}</pre>

That&#8217;s it, that is all the code we need to write.  No need to parse JSON and figure out if we are running locally or in the cloud, all that will be taken care of for us by the Spring Cloud Connectors project.

We do need to do a little more configuration to make sure our application will run locally.  If you had added _spring.data.mongodb.uri_ to your application.properties file you can remove that property, it will no longer be needed.  The Spring Cloud Connectors project uses a separate configuration file for all the cloud services you want to use when running locally.  Somewhere locally on your machine create a file called _spring-cloud.properties_ (you can name it whatever you want if you would like).  I usually put the file in my home directory.  In the file create a property called _spring.cloud.appId_ and set it to any value you would like.  Add another property called _spring.cloud.mongo_ and set its value to _mongodb://localhost:27017_.  If your Mongo DB server is not running on localhost or requires a username and password be sure to make the necessary changes to the URI.  Your properties file should now look like this.

<pre class="font-size:15 lang:default decode:true">spring.cloud.appId: mongo-rest
spring.cloud.mongo: mongodb://localhost:27017</pre>

Save the file if you have not done so already.  Now we need to tell our app about this properties file.  We can do that in <a href="https://github.com/spring-cloud/spring-cloud-connectors/tree/master/spring-cloud-localconfig-connector#property-sources" target="_blank">several ways</a>, but for this example we will specify it in a properties file on the classpath.  In _src/main/resources_ create a new file called _spring-cloud-bootstrap.properties._  In this file add the property _spring.cloud.propertiesFile_ and set it to the path of your _spring-cloud.properties_ file.  If you placed your properties file your home directory you can use the variable _${user.home}_ in the path to represent your home directory.  For example

_spring.cloud.propertiesFile: ${user.home}/spring-cloud.properties_

Once the _spring.cloud.propertiesFile_ is created you should be able to run your application locally and should work just as it did before.  The only difference is that we can now deploy the application to a Cloud Foundry PaaS and as long as there is a Mongo DB service bound to the application it will work there as well.

Now we need to change how we package the application.  I like to be able to setup my POM so I can build both a jar and war file, I find being able to produce a self container jar convenient.  One way to do this is to create a Maven profile to package the application as a war.  Open the POM for the application and add the following XML to the POM.

<pre class="font-size:15 lang:default decode:true ">&lt;packaging&gt;${packaging.type}&lt;/packaging&gt;
  
&lt;properties&gt;
    &lt;packaging.type&gt;jar&lt;/packaging.type&gt; &nbsp;
&lt;/properties&gt;  
&lt;profiles&gt;
  &lt;profile&gt;
      &lt;id&gt;war&lt;/id&gt;
      &lt;properties&gt;
        &lt;packaging.type&gt;war&lt;/packaging.type&gt;
      &lt;/properties&gt;
      &lt;dependencies&gt;
        &lt;dependency&gt;
		  &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
		  &lt;artifactId&gt;spring-boot-starter-tomcat&lt;/artifactId&gt;
		  &lt;scope&gt;provided&lt;/scope&gt;
		&lt;/dependency&gt;
      &lt;/dependencies&gt;
      &lt;build&gt;
	    &lt;finalName&gt;${project.artifactId}&lt;/finalName&gt;
	    &lt;plugins&gt;
	      &lt;plugin&gt;
	        &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
	        &lt;artifactId&gt;spring-boot-maven-plugin&lt;/artifactId&gt;
	      &lt;/plugin&gt;
	      &lt;plugin&gt;
            &lt;artifactId&gt;maven-clean-plugin&lt;/artifactId&gt;
            &lt;version&gt;2.5&lt;/version&gt;
            &lt;executions&gt;
              &lt;execution&gt;
                &lt;id&gt;auto-clean&lt;/id&gt;
                &lt;phase&gt;initialize&lt;/phase&gt;
                &lt;goals&gt;
                  &lt;goal&gt;clean&lt;/goal&gt;
                &lt;/goals&gt;
              &lt;/execution&gt;
            &lt;/executions&gt;
          &lt;/plugin&gt;
	&lt;/plugins&gt;
    &lt;/build&gt;
  &lt;/profile&gt;
&lt;/profiles&gt;</pre>

If you already have a packaging element replace it with the one above.  If you already have a properties element add the new property _packaging.type_ to it.

When the war profile is activated the build will package your application as a war without the Tomcat dependencies.

To get started deploying the Spring Boot application to Bluemix you should login, head to the Catalog, and create <a href="https://console.ng.bluemix.net/#/store/cloudOEPaneId=store&serviceOfferingGuid=fb1a88ad-8006-447c-b0d8-678922fa58d6&fromCatalog=true" target="_blank">a new MongoLab service</a>.  Name the service spring-boot-mongo and leave it unbound for now, we will bind it to the app once we deploy it.  Next package your application by running _$ mvn package -P war_, this should produce a war file in your target directory.  Finally push your application (the following assumes you are in the root of your project).

_$ cf push mongo-demo -p target/mongo-demo.war &#8211;no-start_

_$ cf bind-service mongo-demo spring-boot-mongo_

_$ cf start mongo-demo_

Once your application is deployed test it out by using the REST APIs we defined in the previous posts.  It should work exactly the same locally as it does in the cloud!