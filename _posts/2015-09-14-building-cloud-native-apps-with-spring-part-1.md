---
id: 1103
title: 'Building Cloud Native Apps With Spring- Part 1'
date: 2015-09-14T13:26:33+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1103
permalink: /2015/09/14/building-cloud-native-apps-with-spring-part-1/
dsq_thread_id:
  - 4535002032
dsq_needs_sync:
  - 1
categories:
  - Cloud
  - Cloud Foundry
  - PaaS
---
I had recently written a couple of blog posts about building [cloud native applications](http://ryanjbaxter.com/2015/07/13/building-cloud-native-applications/) and [microservices](http://ryanjbaxter.com/2015/07/15/using-microservices-to-build-cloud-native-applications-part-1/).  Those blog posts were mostly high level overview posts, leaving out the low level implementation details.  This blog post will be a first in a series of blog posts where I will talk about some of my favorite technologies for building cloud native applications and give examples of how to use them.

As you may have noticed by previous [blog posts](http://ryanjbaxter.com/2014/12/08/building-bootiful-java-apps/), I am a fan of <a href="http://projects.spring.io/spring-boot/" target="_blank">Spring Boot</a> and the simplicity it provides when building Java apps.

[<img class="alignnone size-full wp-image-1123" src="http://ryanjbaxter.com/wp-content/uploads/2015/09/spring-boot-logo.png" alt="spring-boot-logo" width="300" height="300" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/09/spring-boot-logo-150x150.png 150w, http://ryanjbaxter.com/wp-content/uploads/2015/09/spring-boot-logo.png 300w" sizes="(max-width: 300px) 100vw, 300px" />](http://ryanjbaxter.com/wp-content/uploads/2015/09/spring-boot-logo.png)

One of the Spring projects which builds upon Boot and that has been rapidly evolving over the past year plus is <a href="http://projects.spring.io/spring-cloud/" target="_blank">Spring Cloud</a>.  The overall goal of the Spring Cloud project is to allow you to build cloud native applications with Spring Boot.  Here is a nice diagram (courtesy of some <a href="http://presos.dsyer.com/decks/cloud-boot-netflix.html" target="_blank">slides</a> from Dave Syer and Spencer Gibb) which explains where Spring Cloud fits in the overall Spring architecture.

[<img class="alignnone size-full wp-image-1122" src="http://ryanjbaxter.com/wp-content/uploads/2015/09/spring-io-tree.png" alt="spring-io-tree" width="920" height="461" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/09/spring-io-tree-300x150.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/09/spring-io-tree.png 920w" sizes="(max-width: 920px) 100vw, 920px" />](http://ryanjbaxter.com/wp-content/uploads/2015/09/spring-io-tree.png)

Some of the features included in Spring Cloud are

  * Distributed/versioned configuration
  * Service registration and discovery\`
  * Routing
  * Service-to-service calls
  * Load balancing
  * Circuit Breakers
  * Global locks
  * Leadership election and cluster state
  * Distributed messaging

As you might be able to tell by this feature list, many of these features have to do with building cloud native apps using microservices.

One of the more interesting projects under the Spring Cloud umbrella is <a href="http://cloud.spring.io/spring-cloud-netflix/" target="_blank">Spring Cloud Netflix</a>.  Spring Cloud Netflix leverages a number of the <a href="https://netflix.github.io/" target="_blank">Netflix OSS projects</a> to provide some of the features listed above.  There are a number of reasons why I find the Spring Cloud Netflix project useful.  First off, Netflix has become the poster child of why microservices is a good way to build cloud applications.  One of the reasons for this is because they have open sourced a lot of code they have written to run one of the biggest, most robust, microservices applications out there under the Netflix OSS umbrella.  This means that the code from Netflix is proven to work in a real world use case, and I always like using code I know works.  To make the Netflix projects easier to use, the Spring team has taken some of these projects and turned them into &#8220;starters&#8221; you can just include in your Spring Boot app, just like you would if you wanted to use JPA or AMQP.  Some of the Spring Cloud Netflix projects are so simple to use that they just require you adding a couple of annotations to your Spring Boot app, the implementation is really nice and clean.  Some of the Netflix OSS projects used in Spring Cloud Netflix include

  * <a href="https://github.com/Netflix/eureka" target="_blank">Eureka</a> &#8211; for service discovery
  * <a href="https://github.com/Netflix/Hystrix" target="_blank">Hystrix</a> &#8211; for all your circuit breaker needs
  * <a href="https://github.com/Netflix/feign" target="_blank">Feign</a> &#8211; allows you to define declarative REST clients
  * <a href="https://github.com/Netflix/ribbon" target="_blank">Ribbon</a>  &#8211; client side load balancing
  * <a href="https://github.com/Netflix/zuul" target="_blank">Zuul</a> &#8211; for routing and filtering

If you would like to learn more about Spring Cloud there are a number of good session recordings that you can listen to.  <a href="https://www.youtube.com/watch?t=3&v=pMPsf9LI8kk" target="_blank">Here</a> is one from <a href="https://twitter.com/starbuxman?lang=en" target="_blank">Josh Long</a> and <a href="https://www.youtube.com/watch?v=BtCSCwT5X5Y" target="_blank">another</a> from <a href="https://twitter.com/spencerbgibb" target="_blank">Spencer Gibb</a>.

Getting started with Spring Cloud is relatively easy, especially if you are already familiar with Spring Boot.  If you head over to <a href="http://start.spring.io/" target="_blank">start.spring.io</a> you will be brought to a page that will basically bootstrap your Spring Boot app just by filling out a form.  The Spring team has integrated the Spring Cloud projects into this tool, allowing you to use them in your Spring Boot app if you choose.  In this blog post, and in a number of follow up posts, we will create a basic microservice app using Spring Boot and Spring Cloud.  One of my interests outside of technology is <a href="https://en.wikipedia.org/wiki/Obstacle_racing" target="_blank">obstacle course racing</a>, so in the spirit of that interest, lets create a web app which lists some upcoming obstacle course races and participants in those races.  There will be three &#8220;services&#8221; that make up the app, one producing the list of races, one which produces the participants in those races, and one that serves clients (browsers) the front-end code.  Lets get started creating the three services.

## Creating The Races Service

First, go to <a href="http://start.spring.io/" target="_blank">start.spring.io</a> and fill out the form like the image below.  The only check box you will need to check off is the one named &#8220;Web&#8221;.

[<img class="alignnone size-full wp-image-1106" src="http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-10.41.47-AM.png" alt="Screen Shot 2015-09-09 at 10.41.47 AM" width="1538" height="610" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-10.41.47-AM-300x119.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-10.41.47-AM-1024x406.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-10.41.47-AM.png 1538w" sizes="(max-width: 1538px) 100vw, 1538px" />](http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-10.41.47-AM.png)

&nbsp;

Then click the Generate button to download the zip file containing the source for your Spring Boot project.  You can then import this project into your favorite IDE, I like to use <a href="https://spring.io/tools" target="_blank">STS</a>, but you can use plain Eclipse or any other Java IDE as well as long as it supports Maven.  There will be one source file in the package _com.ryanjbaxter.spring.cloud.ocr.races_ called _OcrRacesApplication.java_.  Open that up and copy the below code into it.

<pre class="font-size:15 lang:default decode:true">package com.ryanjbaxter.spring.cloud.ocr.races;

import java.util.ArrayList;
import java.util.List;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class OcrRacesApplication implements CommandLineRunner {
	
	private static List&lt;Race&gt; races = new ArrayList&lt;Race&gt;();

    public static void main(String[] args) {
        SpringApplication.run(OcrRacesApplication.class, args);
    }

	@Override
	public void run(String... arg0) throws Exception {
		races.add(new Race("Spartan Beast", "123", "MA", "Boston"));
		races.add(new Race("Tough Mudder RI", "456", "RI", "Providence"));
	}
	
	@RequestMapping("/")
	public List&lt;Race&gt; getRaces() {
		return races;
	}
}

class Race {
	private String name;
	private String id;
	private String state;
	private String city;
	
	public Race(String name, String id, String state, String city) {
		super();
		this.name = name;
		this.id = id;
		this.state = state;
		this.city = city;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getState() {
		return state;
	}
	public void setState(String state) {
		this.state = state;
	}
	public String getCity() {
		return city;
	}
	public void setCity(String city) {
		this.city = city;
	}
}
</pre>

This code is pretty basic, it creates a single REST endpoint which returns all races.  Right now races are just stored in a List in the class, this is just a basic sample, obviously there are more sophisticated ways of doing this  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" /> If you are using STS you can run this app easily by going to Run -> Run As -> Spring Boot Application.  If you prefer you can also start the application via Maven from the command line at the root of the project by running

<pre class="font-size:15 lang:default decode:true ">$ mvn spring-boot:run</pre>

The application will start on localhost using port 8080, so if you open your browser and go to <a href="http://localhost:8080/" target="_blank">http://localhost:8080/</a> you should see a JSON list returned with the race details.

We are going to have many services running at the same time on the same machine and they can&#8217;t all run on the same port, so lets customize the port the races service will run on.  In the _src/main/resources_ directory of the app there will be a file called _application.properties_.  This is where you can set various properties of your Spring app.  I prefer to use YAML files instead (less typing) so rename this file to _application.yml_.  Then open the file and add the following two lines to it.

<pre class="font-size:15 lang:yaml decode:true ">server:
  port: 8282</pre>

Now if you restart your app it should start on port 8282.

## Creating The Participants Service

The next service we want to create is our race participants service.  Again head back to <a href="http://start.spring.io" target="_blank">start.spring.io</a> and fill out the form like the image below.

[<img class="alignnone size-full wp-image-1107" src="http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-11.18.56-AM.png" alt="Screen Shot 2015-09-09 at 11.18.56 AM" width="1543" height="585" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-11.18.56-AM-300x114.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-11.18.56-AM-1024x388.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-11.18.56-AM.png 1543w" sizes="(max-width: 1543px) 100vw, 1543px" />](http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-11.18.56-AM.png)

Click the Generate button to download the code for your project and import it into your IDE.  Again, there will be a single source file in the package _com.ryanjbaxter.spring.cloud.ocr.participants_ called _OcrParticipantsApplication.java_.  Open this file and copy the code below into it.

<pre class="font-size:15 lang:default decode:true">package com.ryanjbaxter.spring.cloud.ocr.participants;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@SpringBootApplication
public class OcrParticipantsApplication implements CommandLineRunner {
	
	private static List&lt;Participant&gt; participants = new ArrayList&lt;Participant&gt;();

    public static void main(String[] args) {
        SpringApplication.run(OcrParticipantsApplication.class, args);
    }

	@Override
	public void run(String... arg0) throws Exception {
		participants.add(new Participant("Ryan", "Baxter", "MA", "S", Arrays.asList("123", "456")));
		participants.add(new Participant("Stephanie", "Baxter", "MA", "S", Arrays.asList("456")));		
	}
	
	@RequestMapping("/")
	public List&lt;Participant&gt; getParticipants() {
		return participants;
	}
	
	@RequestMapping("/races/{id}")
	public List&lt;Participant&gt; getParticipants(@PathVariable String id) {
		return participants.stream().filter(p -&gt; p.getRaces().contains(id)).collect(Collectors.toList());
	}
}

class Participant {
	private String firstName;
	private String lastName;
	private String homeState;
	private String shirtSize;
	private List&lt;String&gt; races;
	public Participant(String firstName, String lastName, String homeState,
			String shirtSize, List&lt;String&gt; races) {
		super();
		this.firstName = firstName;
		this.lastName = lastName;
		this.homeState = homeState;
		this.shirtSize = shirtSize;
		this.races = races;
	}
	public String getFirstName() {
		return firstName;
	}
	public void setFirstName(String firstName) {
		this.firstName = firstName;
	}
	public String getLastName() {
		return lastName;
	}
	public void setLastName(String lastName) {
		this.lastName = lastName;
	}
	public String getHomeState() {
		return homeState;
	}
	public void setHomeState(String homeState) {
		this.homeState = homeState;
	}
	public String getShirtSize() {
		return shirtSize;
	}
	public void setShirtSize(String shirtSize) {
		this.shirtSize = shirtSize;
	}
	public List&lt;String&gt; getRaces() {
		return races;
	}
	public void setRaces(List&lt;String&gt; races) {
		this.races = races;
	}
	
}
</pre>

This class is similar to the same class in the races service, except here we are working with participants.  Again we don&#8217;t want this app to start on port 8080 so in _src/main/resources_ rename _application.properties_ to _application.yml_ and add these two lines.

<pre class="font-size:15 lang:default decode:true ">server:
  port: 8181</pre>

If you start this application and go to <a href="http://localhost:8181/" target="_blank">http://localhost:8181/</a> you will see all participants.  In addition if you go to <a href="http://localhost:8181/races/123" target="_blank">http://localhost:8181/races/123</a> you will see just the participants who will be racing in the race with id 123.

## Creating The Web Service

The final service we are going to create is a service which serves the client-side browser code.  Our web app will be built using <a href="http://angularjs.org" target="_blank">Angular.js</a>.  Again, we will create a new project from <a href="http://start.spring.io" target="_blank">start.spring.io</a>.  Fill out the form following the screen shot below.[<img class="alignnone size-full wp-image-1108" src="http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-2.59.50-PM.png" alt="Screen Shot 2015-09-09 at 2.59.50 PM" width="1549" height="584" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-2.59.50-PM-300x113.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-2.59.50-PM-1024x386.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-2.59.50-PM.png 1549w" sizes="(max-width: 1549px) 100vw, 1549px" />](http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-09-at-2.59.50-PM.png)

Open rename application.properties to application.yml and add the following two lines.

<pre class="font-size:15 lang:default decode:true ">server:
  port: 8080</pre>

In _src/main/resources/static_ create the directories _scripts/controllers_ and _views_.  In _scripts/controllers_ create a new file called _main.js_ and add the following code.

<pre class="lang:default decode:true">angular.module('ocrApp')
  .controller('MainCtrl', function ($scope, $http) {     });</pre>

In the _scripts_ directory create a new file called _app.js_ and add the following code.

<pre class="font-size:15 lang:default decode:true ">angular
  .module('ocrApp', [
    'ngAnimate',
    'ngCookies',
    'ngResource',
    'ngRoute',
    'ngSanitize',
    'ngTouch'
  ])
  .config(function ($routeProvider) {
    $routeProvider
      .when('/', {
        templateUrl: 'views/main.html',
        controller: 'MainCtrl'
      })
      .otherwise({
        redirectTo: '/'
      });
  });</pre>

&nbsp;

In the _views_ directory create a file called _main.html_ and add the following code.

<pre class="font-size:15 lang:default decode:true">&lt;h1&gt;hello world&lt;/h1&gt;</pre>

In the _static_ directory create a new file called _index.html_ and add the following code.

<pre class="font-size:15 lang:default decode:true ">&lt;!DOCTYPE html&gt;
&lt;html lang="en"&gt;
  &lt;head&gt;
    &lt;meta charset="utf-8"&gt;
    &lt;meta http-equiv="X-UA-Compatible" content="IE=edge"&gt;
    &lt;meta name="viewport" content="width=device-width, initial-scale=1"&gt;
    &lt;!-- The above 3 meta tags *must* come first in the head; any other head content must come *after* these tags --&gt;
    &lt;meta name="description" content=""&gt;
    &lt;meta name="author" content=""&gt;
    &lt;link rel="icon" href="../../favicon.ico"&gt;

    &lt;title&gt;OCR Races&lt;/title&gt;

    &lt;!-- Bootstrap core CSS --&gt;
    &lt;!-- Latest compiled and minified CSS --&gt;
	&lt;link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.5/css/bootstrap.min.css"&gt;

    &lt;!-- Custom styles for this template --&gt;
    &lt;link href="http://getbootstrap.com/examples/jumbotron-narrow/jumbotron-narrow.css" rel="stylesheet"&gt;

    &lt;!-- Just for debugging purposes. Don't actually copy these 2 lines! --&gt;
    &lt;!--[if lt IE 9]&gt;&lt;script src="../../assets/js/ie8-responsive-file-warning.js"&gt;&lt;/script&gt;&lt;![endif]--&gt;
    &lt;script src="http://getbootstrap.com/assets/js/ie-emulation-modes-warning.js"&gt;&lt;/script&gt;

    &lt;!-- HTML5 shim and Respond.js for IE8 support of HTML5 elements and media queries --&gt;
    &lt;!--[if lt IE 9]&gt;
      &lt;script src="https://oss.maxcdn.com/html5shiv/3.7.2/html5shiv.min.js"&gt;&lt;/script&gt;
      &lt;script src="https://oss.maxcdn.com/respond/1.4.2/respond.min.js"&gt;&lt;/script&gt;
    &lt;![endif]--&gt;
  &lt;/head&gt;

  &lt;body data-pinterest-extension-installed="cr1.38.4" class=" hasGoogleVoiceExt" ng-app="ocrApp"&gt;

    &lt;div class="container"&gt;
      &lt;div class="header clearfix"&gt;
        &lt;nav&gt;
        &lt;/nav&gt;
        &lt;h3 class="text-muted"&gt;OCR Races&lt;/h3&gt;
      &lt;/div&gt;

      &lt;div ng-view=""&gt;&lt;/div&gt;

      &lt;footer class="footer"&gt;
        &lt;p&gt;© Company 2014&lt;/p&gt;
      &lt;/footer&gt;

    &lt;/div&gt; &lt;!-- /container --&gt;


    &lt;!-- IE10 viewport hack for Surface/desktop Windows 8 bug --&gt;
    &lt;script src="http://getbootstrap.com/assets/js/ie10-viewport-bug-workaround.js"&gt;&lt;/script&gt;
    &lt;script src="//ajax.googleapis.com/ajax/libs/jquery/1.11.3/jquery.min.js"&gt;&lt;/script&gt;
    &lt;script src="//cdnjs.cloudflare.com/ajax/libs/angular.js/1.4.5/angular.js"&gt;&lt;/script&gt;
    &lt;script src="//maxcdn.bootstrapcdn.com/bootstrap/3.3.5/js/bootstrap.min.js"&gt;&lt;/script&gt;
    &lt;script src="//cdnjs.cloudflare.com/ajax/libs/angular.js/1.4.5/angular-animate.js"&gt;&lt;/script&gt;
    &lt;script src="//cdnjs.cloudflare.com/ajax/libs/angular.js/1.4.5/angular-cookies.js"&gt;&lt;/script&gt;
    &lt;script src="//cdnjs.cloudflare.com/ajax/libs/angular.js/1.4.5/angular-resource.js"&gt;&lt;/script&gt;
    &lt;script src="//cdnjs.cloudflare.com/ajax/libs/angular.js/1.4.5/angular-route.js"&gt;&lt;/script&gt;
    &lt;script src="//cdnjs.cloudflare.com/ajax/libs/angular.js/1.4.5/angular-sanitize.js"&gt;&lt;/script&gt;
    &lt;script src="//cdnjs.cloudflare.com/ajax/libs/angular.js/1.4.5/angular-touch.js"&gt;&lt;/script&gt;
    &lt;script src="scripts/app.js"&gt;&lt;/script&gt;
    &lt;script src="scripts/controllers/main.js"&gt;&lt;/script&gt;
  

&lt;/body&gt;&lt;/html&gt;</pre>

If you start this application and go to <a href="http://localhost:8080" target="_blank">http://localhost:8080</a> you will see a simple page that just says hello world.

## Calling Our Races Service

Now it is time to try to leverage some of the services we created in our front-end.  One of the first things we want to do is list all the races.  In the web app service open _main.js_ and add the following code.

<pre class="font-size:15 lang:default decode:true">angular.module('ocrApp')
  .controller('MainCtrl', function ($scope, $http) {
	  $http({
          method: 'GET',
          url: 'http://localhost:8282/races'
      }).then(function(response) {
    	  $scope.races = response.data;
      }, function(response) {
    	  console.error('Error requesting races');
      });
  });</pre>

Here all we are doing is calling our races service to get the list of races and assigning it to a variable in our scope.  Start your races service app and the web app service and go to <a href="http://localhost:8080" target="_blank">http://localhost:8080</a>.  If you open your browsers console you will see the following error.

_Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at http://localhost:8282/races. (Reason: CORS header &#8216;Access-Control-Allow-Origin&#8217; missing)._

If you are a web developer you are probably very familiar with this error.  All modern browsers prevent AJAX requests to other domains unless the server on that domain has specifically allowed requests to come from your domain, this is called the <a href="https://en.wikipedia.org/wiki/Same-origin_policy" target="_blank">same-origin policy</a>.  In this case we are making a request from localhost:8080 to localhost:8282 and the server at localhost:8282 has not said it allows requests coming from localhost:8080.  We could enable <a href="https://en.wikipedia.org/wiki/Cross-origin_resource_sharing" target="_blank">CORS</a> (cross-origin resource sharing) in our races service so we can make requests to it from localhost:8080, but this becomes quite messy.  What happens when we deploy to production or test?  Those are additional domains we have to enable as well.  Since we can theoretically be talking to many, many microservices from the client side code we will have to do this for each service.  In addition, it is not uncommon in a microservices application to have services evolve and change over time, so while the races service is located at a specific URL today, that might not be the case in the future.  In short, hardcoding the URL to the service in our client side code and enabling CORS is just not going to cut it.

Luckily Spring Cloud has a very clean and robust solution available to us.  To solve the problem of hard coding URLs in our client side code, or anywhere in our application, we will want to use service discovery.  Service discovery allows services to query a central location for a complete list of services that are available and the URL(s) those services are available at.  To solve the cross domain problem it would be nice if we had a simple reverse proxy on the same domain as our web app that leveraged the service discovery service to route requests to the right service.  We can use two projects that are part of Spring Cloud Netflix to do just that.  The Spring Cloud Netflix Eureka project will allow us to easily setup a service discovery service for our application while Spring Cloud Netflix Zuul sets up a reverse proxy that integrates with Eureka to call services.  In the next blog post we will take a look at how to integrate these two Spring Cloud projects into our application to solve our cross domain problem.