---
id: 985
title: 'Bluemix Java Developers:  Your life just got a little easier!'
date: 2015-03-02T16:00:52+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=985
permalink: /2015/03/02/bluemix-java-developers-your-life-just-got-a-little-easier/
dsq_thread_id:
  - 4535000918
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
---
&#8230;.well I hope anyways 😉

This week I got around to open sourcing a project I have been working and using for a little while.  The project is called <a href="https://github.com/IBM-Bluemix/bluemix-cloud-connectors" target="_blank">Bluemix Cloud Connectors</a>.  The libraries in this project are meant to make accessing credentials bound to your Java apps in Bluemix a little easier.

What about <a href="https://www.ng.bluemix.net/docs/#starters/liberty/index.html#automaticconfigurationofboundservices" target="_blank">auto-configuration</a> you say?

Well I personally was never a fan of the auto-configuration that happens when you use the Liberty buildpack in Bluemix.  Don&#8217;t get me wrong, it is nice that magic things happen when you deploy your app to Bluemix but what about when you want to test and run your apps locally for development purposes?  I found it hard to reproduce the magic that auto-configuration does in the buildpack locally on my Liberty server.  With the Bluemix Cloud Connectors project you no longer have to rely on the buildpack auto-configuration because it is up to your own app to access the service credentials.  (You want to make sure you <a href="https://www.ng.bluemix.net/docs/#starters/liberty/index.html#automaticconfigurationofboundservices" target="_blank">opt out of auto-configuration</a> if you take this approach.)  The Bluemix Cloud Connectors project encapsulates all the mundane code needed to do this so you end up writing just a couple lines of code in your app.  The best part about this is that the code you write will work locally as well as in Bluemix without having to try and reproduce the auto-configuration magic of the Liberty buildpack.

The Bluemix Cloud Connectors project is based on the <a href="Spring%20Cloud Connectors" target="_blank">Spring Cloud Connectors</a> project (which works with both Spring and non-Spring Java apps).  Out of the box the Spring Cloud Connectors project has support for the following services:

<ul class="task-list">
  <li>
    PostgreSQL
  </li>
  <li>
    MySQL
  </li>
  <li>
    Oracle
  </li>
  <li>
    Redis
  </li>
  <li>
    MongoDB
  </li>
  <li>
    RabbitMQ
  </li>
  <li>
    SMTP gateway
  </li>
  <li>
    application monitoring (New Relic)
  </li>
</ul>

The Bluemix Cloud Connectors project adds support for a couple of additional services in Bluemix:

  * Cloudant
  * SendGrid
  * Twilio

Supporting other services in Bluemix is certainly possible but right now those were the only three I needed to use for my own project 😉  If you would like support for other services please open an <a href="https://github.com/IBM-Bluemix/bluemix-cloud-connectors/issues" target="_blank">issue on GitHub</a>.

So how easy is it to use these libraries?  Just do the following

Add these dependencies to your POM

<pre class="font-size:15 lang:default decode:true ">&lt;dependency&gt;
      &lt;groupId&gt;net.bluemix&lt;/groupId&gt;
      &lt;artifactId&gt;bluemix-cloud-connectors-cloudfoundry&lt;/artifactId&gt;
      &lt;version&gt;0.0.1.RC2&lt;/version&gt;
    &lt;/dependency&gt;
    &lt;dependency&gt;
      &lt;groupId&gt;net.bluemix&lt;/groupId&gt;
      &lt;artifactId&gt;bluemix-cloud-connectors-local&lt;/artifactId&gt;
      &lt;version&gt;0.0.1.RC2&lt;/version&gt;
    &lt;/dependency&gt;</pre>

If you are building a Spring app you will also have to add

<pre class="font-size:15 lang:default decode:true">&lt;dependency&gt;
      &lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
      &lt;artifactId&gt;spring-cloud-spring-service-connector&lt;/artifactId&gt;
      &lt;version&gt;1.1.0.RELEASE&lt;/version&gt;
    &lt;/dependency&gt;</pre>

If you are building a JEE app you will need to use the CloudFactory class.  Here is an example for Cloudant.

<pre class="font-size:15 lang:default decode:true ">import javax.enterprise.context.ApplicationScoped;
import javax.enterprise.inject.Produces;

import org.ektorp.CouchDbInstance;
import org.ektorp.impl.StdCouchDbConnector;
import org.springframework.cloud.Cloud;
import org.springframework.cloud.CloudFactory;

@ApplicationScoped
public class CloudantBean {
  private static Cloud cloud = new CloudFactory().getCloud();
  @Produces
  public StatusRepository statusRepository() {
    CouchDbInstance db = cloud.getServiceConnector("connectors-sample", CouchDbInstance.class, null /* default config */);
    return new StatusRepository(new StdCouchDbConnector("status", db));
  }
}</pre>

If you are building a Spring app things are even simpler.  Again here is an example for Cloudant.

<pre class="font-size:15 lang:default decode:true ">import javax.naming.NamingException;

import org.ektorp.CouchDbInstance;
import org.springframework.cloud.config.java.AbstractCloudConfig;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;


public class Config {
  @Configuration
  static class CloudConfiguration extends AbstractCloudConfig {
    @Bean
    public CouchDbInstance couchDbInstance() throws NamingException {
      CouchDbInstance instance = connectionFactory().service(CouchDbInstance.class);
      return instance;
    }
  }
}</pre>

To get this code to work locally you will need to add a file called _spring-cloud-bootstrap.properties_ to your application&#8217;s classpath.  In that file you should have a single property called _spring.cloud.propertiesFile_ that points to another properties file on your local file system.  This file should contain properties for the &#8220;services&#8221; your application uses locally.  To make the code above work the properties file should look like this

<pre class="font-size:15 lang:default decode:true  ">spring.cloud.appId: cloudant-sample
spring.cloud.connectors-sample: couchdb://user:password@localhost:5984</pre>

Want to check out the code?  No problem, it is all up on <a href="https://github.com/IBM-Bluemix/bluemix-cloud-connectors" target="_blank">GitHub</a>.

Want working samples?  No problem, check out the <a href="https://github.com/IBM-Bluemix/bluemix-cloud-connectors/tree/master/samples" target="_blank">samples directory</a> for both a JEE sample that will run on Liberty and a Spring Boot app.  Be sure to read through the READMEs of both samples for details on how to run them locally as well as deploy them to Bluemix.

Enjoy!

&nbsp;

&nbsp;