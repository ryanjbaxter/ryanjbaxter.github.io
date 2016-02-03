---
id: 915
title: 'Building &#8220;Bootiful&#8221; Java Apps'
date: 2014-12-08T14:57:53+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=915
permalink: /2014/12/08/building-bootiful-java-apps/
dsq_thread_id:
  - 4535001924
categories:
  - Uncategorized
---
A few months ago when I started to learn Node.js I came across code that looked like this

<pre class="font-size:15 lang:js decode:true" title="Node.js HTTP Server">var http = require('http');
http.createServer(function (req, res) { 
  res.writeHead(200, {'Content-Type': 'text/plain'}); 
  res.end('Hello World');
}).listen(3000);</pre>

If you are not familiar with JavaScript, this code starts an HTTP server running on localhost at port 3000 and when you hit http://localhost:3000 it returns the string &#8220;Hello World&#8221;.

This code isn&#8217;t really impressive in functionality, it just returns a string, but what is impressive is how easy it was to get to that point, it only took 5 lines of code.  I am a long time Java developer, and as any Java developer knows doing the same thing in Java with such little code and effort is a near impossibility.

Consider how you would do this with JEE.  The servlet 3 spec lets you easily define routes in your app using annotations, so that code would be fairly small, but you would also need a web.xml as well.  With the Java class and the web.xml you are probably already over 5 lines of code and you don&#8217;t even have a web server yet!

I had just started looking into Node.js and I already could see why people like languages like Node.js and Ruby when compared to something like Java.  However I wasn&#8217;t ready to abandon just Java yet, so I decided to look around to see if anyone had written a Java framework that would let me recreate the hello world Node.js app above with just as little code and effort.

After some Googling I came across a project called <a href="http://spring.io/guides/gs/spring-boot/" target="_blank">Spring Boot</a>.  As I read through some of the documentation I came across this code

<pre class="font-size:15 lang:java decode:true">@RestController
class ThisWillActuallyRun { 
  @RequestMapping("/") 
  String home() {  
    return "Hello World!" 
  }
}</pre>

The documentation promised that this code would start a web server and return Hello World when the root of the web application is hit in a browser.  Now there is one caveat to this, the above code is actually Groovy and not Java.  The Groovy syntax is very similar and the language runs on the JVM, so while it did not achieve my goal of being pure Java, it peaked my interest.

So how do you run the above code?  Save the code to a file called app.groovy.  Then download the <a href="http://docs.spring.io/spring-boot/docs/1.1.9.RELEASE/reference/htmlsingle/#getting-started-installing-the-cli" target="_blank">Spring Boot CLI</a> and then run

<pre class="font-size:15 lang:default decode:true">$ spring run app.groovy</pre>

And like that you have an HTTP server running on port 8080 and if you hit http://localhost:8080 you will see &#8220;Hello World!&#8221;.  I was shocked, that was super easy&#8230;.but it gets better!

You can then run

<pre class="font-size:15 lang:default decode:true">$ spring jar app.jar app.groovy</pre>

To package your app into a jar file.  Want to run the app?  It&#8217;s as simple as running

<pre class="font-size:15 lang:default decode:true">$ java -jar app.jar</pre>

And like that your app is running again&#8230;.and yes that means the web server is included in the jar!!!  Whats even more impressive about what happened is you can now take that jar and give it to anyone and they can run it, all they need is to have is a JVM installed.  No more making sure you have the right app container installed and configured properly, its all inside the jar file.

At this point you are probably saying &#8220;OK Ryan, this is pretty cool but this is Groovy and not Java so how does this make Java apps easier?&#8221;  Good question 😉

The good news is that the same functionality works in Java, the only difference is it requires slightly more code, but not much.  Here is what the same code looks like in Java.

<pre class="font-size:15 lang:default decode:true">import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;

@Configuration
@ComponentScan
@EnableAutoConfiguration
@RestController
public class Application {
  public static void main(String[] args) {
    SpringApplication.run(Application.class, args);
  }

  @RequestMapping("/")
  public String hello() {
    return "Hello World!";
  }
}</pre>

Even with the import statements and the annotations this code is relatively small.  So what is happening here?

@Configuration is telling Spring that there could be some configuration in the class.

@ComponentScan is telling Spring to look in the package containing this class and any subpackages for anything considered a component.  @RestController (see below) is an example of a component but the details of a component are not really important right now.

@EnableAutoConfiguration is a pretty important annotation.  It basically tells Spring to try to automatically configure everything your can without the developer having to do anything.

Lastly @RestController tells Spring that this class is going to have REST APIs in it.

The next thing you will probably notice is the public static void main method.  This is what kicks off the application, and also means you can run this application just like you would run any other public static void main application.  More on this in a little bit.

Finally, we have the hello method.  The code here is pretty simple, it just returns the string &#8220;Hello World!&#8221;, nothing complicated.  The @RequestMapping annotation on the hello method works with the @RequestController annotation.  It says when a request is made to the root of the web app, call the hello method.

Lastly, we need some type of build framework to package our application, Spring Boot integrates nicely with either Maven or Gradle.  In this example we will use Maven.  Here is what our Maven POM looks like.

<pre class="font-size:15 lang:default decode:true">&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"&gt;
  &lt;modelVersion&gt;4.0.0&lt;/modelVersion&gt;
  &lt;groupId&gt;org.test&lt;/groupId&gt;
  &lt;artifactId&gt;demo&lt;/artifactId&gt; 
  &lt;version&gt;0.0.1-SNAPSHOT&lt;/version&gt; 
  &lt;packaging&gt;jar&lt;/packaging&gt; 
  &lt;name&gt;demo&lt;/name&gt;
  &lt;description&gt;Demo project for Spring Boot&lt;/description&gt;

  &lt;parent&gt;
    &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
    &lt;artifactId&gt;spring-boot-starter-parent&lt;/artifactId&gt;
    &lt;version&gt;1.1.9.RELEASE&lt;/version&gt;
    &lt;relativePath/&gt; &lt;!-- lookup parent from repository --&gt;
  &lt;/parent&gt;

  &lt;dependencies&gt;
    &lt;dependency&gt;
      &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
      &lt;artifactId&gt;spring-boot-starter-web&lt;/artifactId&gt;
    &lt;/dependency&gt;
  &lt;/dependencies&gt;

  &lt;build&gt;
    &lt;plugins&gt;
      &lt;plugin&gt;
        &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
        &lt;artifactId&gt;spring-boot-maven-plugin&lt;/artifactId&gt;
      &lt;/plugin&gt;
    &lt;/plugins&gt;
  &lt;/build&gt; 
&lt;/project&gt;
</pre>

Most of this should be pretty standard if you are familiar with Maven, but here are the important parts.

All Spring Boot Maven projects should inherit from the Spring Boot parent POM.  The Spring Boot parent POM contains what Spring Boot calls &#8220;<a href="http://docs.spring.io/spring-boot/docs/1.1.9.RELEASE/reference/htmlsingle/#using-boot-starter-poms" target="_blank">starter dependencies</a>&#8221;  These started dependencies usually contain a number of jars from other projects to provide some type of easily integrated set of functionality.  For example if you want to use a Mongo DB in your application you would include the Mongo DB starter dependency.  Your application would then get everything it needs to integrate with Mongo DB.  This makes it easier for you as a developer as you don&#8217;t need to search around for which jars you need and deal with getting the right versions of those jars for the other dependencies you may have, the starter dependencies take care of all that for you.

Next is the spring-boot-starter-web dependency (another example of a starter dependency).  This dependency gives us everything we need to build a Java web app, including things like the @RestController and @RequestMapping annotations.

Finally we have the Spring Boot Maven build plugin.  This plugin takes care of packaging your application up for you.

So how do we run this app?  It&#8217;s easy, its just like any other Maven project.  Just run the following command from the directory containing the POM file.

<pre class="font-size:15 lang:default decode:true">$ mvn package</pre>

This produces a jar file in the target directory of your app.  This might seem odd initially, usually Java web apps are packaged as wars and you usually take the war and drop it into an app container like Tomcat, Jetty, or Websphere.  Not necessary with Spring Boot, to run your application you just run

<pre class="font-size:15 lang:default decode:true">$ java -jar target/demo-0.0.1-SNAPSHOT.jar</pre>

When you do that, an HTTP server will start your web app for you and you can go to http://localhost:8080 in your browser and see &#8220;Hello World!&#8221;.

So how does this work?  Where does the HTTP server come from?  Well besides the @RequestMapping and @RestController annotations, the spring-boot-starter-web dependency provides an embedded Tomcat server.  The Spring Boot build plugin does some magic packaging and takes all the dependencies and repackages everything into one jar with your applications code.  That combined with the fact that there is a public static void main class means you can just run the jar like you would any other jar.  Check out this video showing how to build and run the hello world Spring Boot app.

&nbsp;

&nbsp;

<iframe width="420" height="315" src="https://www.youtube.com/embed/8oWsdto5mpU" frameborder="0" allowfullscreen></iframe>

&nbsp;

I hope this blog post has peaked your interest in Spring Boot.  This was the easiest most elegant way I had ever seen to build a Java web app, and it truly was &#8220;bootiful&#8221;.  I quickly discovered that this was just the tip of the ice berg, there is so much more Spring Boot has to offer.  I have pretty much been using Spring Boot exclusively for my Java development over the past few months and I have been pleasantly pleased with it.  Over the next few weeks I will continue to blog and show you some of the cool things Spring Boot can do, including how it will change the way you build applications for the Cloud.  Stay tuned!

&nbsp;

**NOTE:** I shamelessly stole the term &#8220;Bootiful&#8221; from <a href="https://twitter.com/starbuxman" target="_blank">Josh Long</a>.

&nbsp;

&nbsp;

&nbsp;

&nbsp;

&nbsp;