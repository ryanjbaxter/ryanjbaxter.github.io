---
id: 1162
title: 'Building Cloud Native Apps With Spring &#8211; Part 5'
date: 2015-10-12T13:16:06+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1162
permalink: /2015/10/12/building-cloud-native-apps-with-spring-part-5/
dsq_thread_id:
  - 4535074710
categories:
  - Cloud
  - PaaS
  - Spring
---
In this next section of the tutorial we will discuss a very important topic when it comes to microservice apps, circuit breakers.

[<img class="alignnone size-full wp-image-1178" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/circuit_breaker.jpg" alt="circuit_breaker" width="500" height="375" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/circuit_breaker-300x225.jpg 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/circuit_breaker.jpg 500w" sizes="(max-width: 500px) 100vw, 500px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/circuit_breaker.jpg)

One of the inherent problems when you have a distributed app like a microservice app is cascading failures across services.  Since the application is composed of several distributed services, there are more chances for failure when making remote calls.  In addition, distributed apps often tend to chain together service calls, so any problem with a service at the end of the chain can cause problems for all the services further up the chain.

As a service owner, we want to insulate ourselves from problems in our dependent services, so how do we do that?  One solution is to use the [circuit breaker pattern](http://techblog.netflix.com/2011/12/making-netflix-api-more-resilient.html) which was introduced by Michael Nygard in his book <a href="http://www.amazon.com/gp/product/0978739213?ie=UTF8&tag=martinfowlerc-20&linkCode=as2&camp=1789&creative=9325&creativeASIN=0978739213" target="_blank">Release It</a>.

[<img class="alignnone size-full wp-image-1179" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/download.jpeg" alt="download" width="205" height="246" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/download.jpeg)

In the circuit breaker pattern, calls to remote services are protected by what is called a circuit breaker.  As long as there are no errors, the circuit stays closed and the remote calls are made as normal.  If the circuit detects a problem with the remote call than the circuit breaker is tripped and the circuit is opened stopping the remote call from being made.  Once the circuit detects that the remote call can successfully be made again the circuit will be closed allowing the remote call to be made.

In the Netflix and Spring Cloud world, the tool for implementing circuit breakers is called <a href="http://cloud.spring.io/spring-cloud-netflix/spring-cloud-netflix.html#_circuit_breaker_hystrix_clients" target="_blank">Hystrix</a>.  Lets look at how we protect the remote service calls in our app using Hystrix.

## Adding Circuit Breakers To Our Code

There are 2 places in our app where one service is calling another service.  The first place that should come to mind is where we are using the Zuul proxy (from <a href="http://ryanjbaxter.com/2015/09/21/building-cloud-native-apps-with-spring-part-2/" target="_blank">part 2</a> of this tutorial).  The Zuul proxy is used in our web app to proxy our JavaScript calls from our web app to the other microservices.  Luckily Spring Cloud automatically protects all these calls with circuit breakers for you so there is nothing we really have to do <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

The other remote call we make in our app involves the API we added for mobile clients in the <a href="http://ryanjbaxter.com/2015/10/01/building-cloud-native-apps-with-spring-part-4/" target="_blank">last blog post</a>.  In this API we make a request to our Participants service from our Races service.  If for whatever reason our Participants service is down or not responding fast enough, our Races service will suffer.  Rather than have our Races service break because of issues with the Participants service, we can protect the remote call with a circuit breaker and actually return something back to our clients even if there is a problem with the Participants service.  What we respond back with might not be ideal or have all the information our clients need, but it is better than the call failing or timing out.  Lets take a look at how we use Hystrix in our Races service.

In order to use Hystrix we need to add the Hystrix dependency to our Races service.  Open the POM file for the Races service and add the following dependency in the dependencies section.

<pre class="font-size:15 lang:default decode:true ">&lt;dependency&gt;
  &lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
  &lt;artifactId&gt;spring-cloud-starter-hystrix&lt;/artifactId&gt;
&lt;/dependency&gt;</pre>

Now that we have our dependency in place, we can start using Hystrix.  In our <a href="http://ryanjbaxter.com/2015/10/01/building-cloud-native-apps-with-spring-part-4/" target="_blank">last blog post</a> we created a Feign client to make remote calls to the Participants service and added a new REST API at _/participants_ which used the Feign client to get participant information and add it to the race data.  We need to protect this call from potential failures, so the obvious solution would be to add a circuit breaker around the REST API.  Unfortunately, right now we cannot wrap a REST Controller in a circuit breaker (see this <a href="https://github.com/spring-cloud/spring-cloud-netflix/issues/207" target="_blank">GitHub issue</a>).  Because of this limitation we will need to break out the call to the Feign client into its own Bean.  Open _OcrRacesApplication.java_ and update the _OcrRacesApplication_ class and add a new bean called _ParticipantsBean_.

&nbsp;

<pre class="font-size:15 lang:default decode:true">@SpringBootApplication
@RestController
@EnableEurekaClient 
@EnableFeignClients
@EnableCircuitBreaker
public class OcrRacesApplication implements CommandLineRunner {
	
	private static List&lt;Race&gt; races = new ArrayList&lt;Race&gt;();
	@Autowired
	private ParticipantsBean participantsBean;

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
	
	@RequestMapping("/participants")
	public List&lt;RaceWithParticipants&gt; getRacesWithParticipants() {
		List&lt;RaceWithParticipants&gt; returnRaces = new ArrayList&lt;RaceWithParticipants&gt;();
		for(Race r : races) {
			returnRaces.add(new RaceWithParticipants(r, participantsBean.getParticipants(r.getId())));
		}
		return returnRaces;
	}
}  

 @Component
class ParticipantsBean {
	@Autowired
	private ParticipantsClient participantsClient;
	
	@HystrixCommand(fallbackMethod = "defaultParticipants")
	public List&lt;Participant&gt; getParticipants(String raceId) {
		return participantsClient.getParticipants(raceId);
	}
	
	public List&lt;Participant&gt; defaultParticipants(String raceId) {
		return new ArrayList&lt;Participant&gt;();
	}
}</pre>

In the _OcrRacesApplication_ class we have added the _@EnableCircuitBreake_r annotation to enable circuit breakers in our application.  The next change to this class is in our _/participants_ API where we are now calling our new bean instead of the Feign client directly.  In the new bean we just wrap the call to the Feign client in a method called _getParticipants_.  This is the method we wrap in a circuit breaker since it is the one using the remote service.  We enable the circuit breaker functionality by using the _@HystrixCommand_ annotation on the method.  In the annotation we specify a fallback method to call if the circuit is open.  If the circuit is open we call the method in our bean called _defaultParticipants_ which just returns an empty list.  You can do whatever you want in your fallback method, and generally it would be more sophisticated than returning an empty list, but for this sample an empty list is good enough.  In a production application, maybe our Races services would cache participants data so we have something to return if the circuit is ever open.

That is all we have to do, now our remote call to the Participants service is protected by a circuit breaker.

## Hystrix Dashboard

Having circuit breakers in our services is nice, but how do we know the state of the circuits?  Luckily Netflix and Spring Cloud provide a web application called the <a href="http://cloud.spring.io/spring-cloud-netflix/spring-cloud-netflix.html#_circuit_breaker_hystrix_dashboard" target="_blank">Hystrix Dashboard</a> that gives us the information we need.  This dashboard gives developers and operations insight into various statistics about the circuits in their applications such as success and failure rates.  In addition to the Hystrix Dashboard, Netflix and Spring Cloud also offer another tool called <a href="http://cloud.spring.io/spring-cloud-netflix/spring-cloud-netflix.html#_turbine" target="_blank">Turbine</a>.  Turbine helps aggregate various streams of Hystrix data into a single stream so you don&#8217;t have to continuously switch streams in the dashboard to view data from different instances of a service.

To take advantage of these tools in our application, lets add a new service to our app to host them.  Go to <a href="http://start.spring.io" target="_blank">start.spring.io</a> and create a new project based on the following image.

&nbsp;

[<img class="alignnone size-full wp-image-1166" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-9.16.21-AM.png" alt="Screen Shot 2015-10-07 at 9.16.21 AM" width="1202" height="863" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-9.16.21-AM-300x215.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-9.16.21-AM-1024x735.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-9.16.21-AM.png 1202w" sizes="(max-width: 1202px) 100vw, 1202px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-9.16.21-AM.png)

&nbsp;

Make sure you add the Hystrix Dashboard and Turbine starters.  Once you have the form filled out, click Generate Project to download the zip and import the project into your workspace.  To enable the Hystrix Dashboard, we need to add a single annotation in _com.ryanjbaxter.spring.cloud.ocr.hystrix.HystrixDashboardApplication_.  Open this class file and add _@EnableHystrixDashboard_ to the class file.

<pre class="font-size:15 lang:default decode:true">package com.ryanjbaxter.spring.cloud.ocr.hystrix;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.hystrix.dashboard.EnableHystrixDashboard;
import org.springframework.cloud.netflix.turbine.EnableTurbine;

@SpringBootApplication
@EnableHystrixDashboard 
public class HystrixDashboardApplication {

    public static void main(String[] args) {
        SpringApplication.run(HystrixDashboardApplication.class, args);
    }
}</pre>

The only other thing we have to do now is a little configuration.  We want to change the port our Hystrix Dashboard and Turbine services will be running on so go to _src/main/resources_ in the new project and rename _application.properties_ to _application.yml_.  Then add the following properties to the YAML file.

<pre class="font-size:15 lang:default decode:true ">server:
  port: 8383</pre>

Start the application, which will be running on port 8383, and go to <a href="http://localhost:8383/hystrix" target="_blank">http://localhost:8383/hystrix</a>.  You should see a page that looks like this.[<img class="alignnone size-full wp-image-1167" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-9.28.44-AM.png" alt="Screen Shot 2015-10-07 at 9.28.44 AM" width="850" height="597" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-9.28.44-AM-300x211.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-9.28.44-AM.png 850w" sizes="(max-width: 850px) 100vw, 850px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-9.28.44-AM.png)

The one required field in this form is a URL to either a Hystrix or Turbine stream.  We have not yet configured Turbine, so lets try a Hystrix stream.  Start up all the other services for the app (Eureka, Races, Participants, and Web) and wait for everything to register itself with Eureka.

Once everything is registered go to the web app at <a href="http://localhost:8080" target="_blank">http://localhost:8080</a> and click on a race to view the participants.  This step is necessary in order to see any interresting statistics regarding the circuit breakers in Zuul.  Now go back to your Hystrix Dashboard, enter the URL _http://localhost:8080/hystrix.stream,_ and click Monitor Stream.  The resulting dashboard should look something like the screen shot below.

[<img class="alignnone size-full wp-image-1168" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-9.55.14-AM.png" alt="Screen Shot 2015-10-07 at 9.55.14 AM" width="1117" height="251" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-9.55.14-AM-300x67.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-9.55.14-AM-1024x230.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-9.55.14-AM.png 1117w" sizes="(max-width: 1117px) 100vw, 1117px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-9.55.14-AM.png)

You will notice we have 2 circuit breakers, one for the call to proxy requests to the Races service, and the other for the call to proxy requests to the Participants service.  If you start refreshing the web app you will notice the dashboard change as it monitors requests through the circuit breakers.  However, you typically cannot do a very good job simulating load on a service by refreshing a page manually in a browser.  One tool that can better simulate load on our services is called [Siege](https://www.joedog.org/siege-manual/).  You can install Siege via your favorite package manager (<a href="http://brewformulas.org/Siege" target="_blank">Homebrew</a>, <a href="http://damien.co/resources/web-application-testing-install-siege-on-linux-ubuntu-or-mac-osx-6011" target="_blank">Yum, Apt-Get</a>, etc).  Once installed it is pretty easy to <a href="https://www.joedog.org/siege-manual/" target="_blank">use</a>.  For example, to hit the Races service through our Zuul proxy you would just do

<pre class="font-size:15 lang:default decode:true">$ siege http://localhost:8080/races</pre>

Once you do this, take a look at the Hystrix dashboard and you will notice the some more interresting data in the dashboard.

[<img class="alignnone size-full wp-image-1169" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.05.21-AM.png" alt="Screen Shot 2015-10-07 at 10.05.21 AM" width="1114" height="241" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.05.21-AM-300x65.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.05.21-AM-1024x222.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.05.21-AM.png 1114w" sizes="(max-width: 1114px) 100vw, 1114px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.05.21-AM.png)

&nbsp;

For information about what all the numbers mean in the dashboard take a look at the <a href="https://github.com/Netflix/Hystrix/wiki/Dashboard" target="_blank">Hystrix wiki page</a> on GitHub.

What about monitoring the circuit breaker we added in the Races service?  First, lets make sure we hit the API so we have some data.  Go to <a href="http://localhost:8282/participants" target="_blank">http://localhost:8282/participants</a>.  Then back on the Hystrix Dashboard landing page <a href="http://(http://localhost:8383/hystrix" target="_blank">(http://localhost:8383/hystrix</a>) enter the URL _http://localhost:8282/hystrix.stream_ and click Monitor Stream.  When you do this you should get an error in the dashboard, why?

[<img class="alignnone size-full wp-image-1170" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.38.23-AM.png" alt="Screen Shot 2015-10-07 at 10.38.23 AM" width="1120" height="229" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.38.23-AM-300x61.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.38.23-AM-1024x209.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.38.23-AM.png 1120w" sizes="(max-width: 1120px) 100vw, 1120px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.38.23-AM.png)

This is because the stream functionality hasn&#8217;t been enabled in the application yet (Zuul automatically has it enabled, that is why it worked out of the box in our Web service).  To add the Hystrix Stream to the app we need to add the <a href="https://spring.io/guides/gs/actuator-service/" target="_blank">Spring Boot Actuator</a> starter to the Races service (as specified in the Spring Cloud Netflix <a href="http://cloud.spring.io/spring-cloud-netflix/spring-cloud-netflix.html#_hystrix_metrics_stream" target="_blank">documentation</a>).  Open the POM file for the Races service and add the following dependency in the dependencies section.

<pre class="font-size:15 lang:default decode:true ">&lt;dependency&gt;
  &lt;groupId&gt;org.springframework.boot&lt;/groupId&gt;
  &lt;artifactId&gt;spring-boot-starter-actuator&lt;/artifactId&gt;
&lt;/dependency&gt;</pre>

Save the POM and restart the Races service.  Again hit the _/participants_ API in your browser by going to <a href="http://localhost:8282/participants" target="_blank">http://localhost:8282/participants</a>.  Now back on the Hystrix Dashboard landing page enter _http://localhost:8282/hystrix.stream_ and click Monitor Stream.  Now you should see information about our _getParticipants_ method protected by our circuit breaker.

[<img class="alignnone size-full wp-image-1171" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.53.26-AM.png" alt="Screen Shot 2015-10-07 at 10.53.26 AM" width="1117" height="394" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.53.26-AM-300x106.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.53.26-AM-1024x361.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.53.26-AM.png 1117w" sizes="(max-width: 1117px) 100vw, 1117px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.53.26-AM.png)

Again if you put the API under seige you will start to see more interesting data.  But what happens in the failure case?  If you shut down the Participants service, just by stopping it from running, and then hit the API or put the API under seige you should see the circuit open and the number of failures in the dashboard go up.

[<img class="alignnone size-full wp-image-1174" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-1.06.21-PM.png" alt="Screen Shot 2015-10-07 at 1.06.21 PM" width="610" height="384" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-1.06.21-PM-300x189.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-1.06.21-PM.png 610w" sizes="(max-width: 610px) 100vw, 610px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-1.06.21-PM.png)

In the above screenshot we see a number of requests have started failing (the purple number) and our error rate is starting to go up however the circuit is still closed.  In the below screenshot the circuit has finally opened due to the number of failing requests

&nbsp;

[<img class="alignnone size-full wp-image-1172" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.57.39-AM.png" alt="Screen Shot 2015-10-07 at 10.57.39 AM" width="1119" height="244" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.57.39-AM-300x65.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.57.39-AM-1024x223.png 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.57.39-AM.png 1119w" sizes="(max-width: 1119px) 100vw, 1119px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-10.57.39-AM.png)

The number in blue (548) is the number of calls that have been short-circuited or have gone to our fallback method defined in our _@HystrixCommand_.  Since the circuit is open, if we hit the API in the browser we should see empty lists come back for the participants data since that is the behavior we defined in our fallback method.  Give it a shot, go to <a href="http://localhost:8282/participants" target="_blank">http://localhost:8282/participants</a>.  Notice the data returned will look like this

<pre class="font-size:15 lang:default decode:true ">[
   {
      "name":"Spartan Beast",
      "id":"123",
      "state":"MA",
      "city":"Boston",
      "participants":[

      ]
   },
   {
      "name":"Tough Mudder RI",
      "id":"456",
      "state":"RI",
      "city":"Providence",
      "participants":[

      ]
   }
]</pre>

No participant data as expected.

Now if we start the Participants service back up the circuit should close and our requests should again succeed.  But how does the circuit know the service is back up?  Periodically the circuit will let a couple of requests through to see if they succeed or not.  Notice the 2 failures (in red) in the screenshot below.  When these requests starts succeeding then the circuit will be closed and the requests will be let through.

[<img class="alignnone size-full wp-image-1175" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-1.10.50-PM.png" alt="Screen Shot 2015-10-07 at 1.10.50 PM" width="682" height="410" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-1.10.50-PM-300x180.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-1.10.50-PM.png 682w" sizes="(max-width: 682px) 100vw, 682px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-1.10.50-PM.png)

Now that the service is back up everything gets back to normal and we see the circuit close.

[<img class="alignnone size-full wp-image-1176" src="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-1.17.17-PM.png" alt="Screen Shot 2015-10-07 at 1.17.17 PM" width="672" height="360" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-1.17.17-PM-300x161.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-1.17.17-PM.png 672w" sizes="(max-width: 672px) 100vw, 672px" />](http://ryanjbaxter.com/wp-content/uploads/2015/10/Screen-Shot-2015-10-07-at-1.17.17-PM.png)

&nbsp;

## Using Turbine

This is all nice, but switching between various Hystrix Streams can be a pain, it would be nice to be able to see streams based on service id.  This is where Turbine comes in.  When we created our Hystrix Dashboard project on start.spring.io we added the Turbine dependency to our project so we have all the dependencies we need already in our POM.  To enable Turbine we need to add a single annotation to _com.ryanjbaxter.spring.cloud.ocr.hystrix.HystrixDashboardApplication_.  Open the class file and add _@EnableTurbine_

<pre class="font-size:15 lang:default decode:true ">package com.ryanjbaxter.spring.cloud.ocr.hystrix;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.hystrix.dashboard.EnableHystrixDashboard;
import org.springframework.cloud.netflix.turbine.EnableTurbine;

@SpringBootApplication
@EnableHystrixDashboard
@EnableTurbine
public class HystrixDashboardApplication {

    public static void main(String[] args) {
        SpringApplication.run(HystrixDashboardApplication.class, args);
    }
}
</pre>

Now we need to tell Turbine which services we want it to aggregate the information for.  Open _src/main/resources/application.yml_ and add the following configuration properties.

<pre class="font-size:15 lang:default decode:true ">server:
  port: 8383
  
turbine:
  appConfig: web,races
  aggregator:
    clusterConfig: WEB,RACES</pre>

The _turbine.appConfig_ property specifies the service names that you would like to aggregate.  These values just need to match the service IDs we have already configured with Eureka.  The _turbine.aggregator.clusterConfig_ parameter are cluster names and they need to match the service IDs as well, the only difference being that they MUST be uppercase.

After adding the new annotation and the additional config restart the Hystrix Dashboard app.  Once the app is back up, head to the Hystrix Dashboard again (<a href="http://localhost:8383/hystrix" target="_blank">http://localhost:8383/hystrix</a>).  Now instead of entering URLs to the individual Hystrix streams of our services, lets use Turbine.  Enter the URL <a href="http://localhost:8383/turbine.stream?cluster=WEB" target="_blank">http://localhost:8383/turbine.stream?cluster=WEB</a> and click Monitor Stream.  This should bring up all the circuit breakers in the Web service (the one using our Zuul proxy).  You should see the circuit breakers for the Participants and Races routes displayed in the dashboard just like if you were monitoring the Hystrix Stream of the individual service.

What if we want to look at the circuit breakers for the Races service?  All we have to do is adjust the cluster query parameter in the Turbine URL to point to the Races cluster.  Go back to the Hystrix landing page by either clicking the back button in your browser or going to back to <a href="http://localhost:8383/hystrix" target="_blank">http://localhost:8383/hystrix</a>.  Now enter the same Turbine URL but with the Races query parameter, _http://localhost:8383/turbine.stream?cluster=RACES_.  Again what you will see should be the same as if we were pointing at the Races Hystrix stream.  The obvious benefit here is we can monitor Hystrix streams based on service ID as opposed to URL.  The other benefit, which is less obvious, is that if we were to scale these services horizontally we would see statistics about all instances of that service instead of just an individual service.  This will be more useful when our application is running in the cloud&#8230;don&#8217;t worry we will get there eventually <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />

As you can see, circuit breakers play a very important role in a microservices application, and as usual Spring Cloud makes it easy to not only use them in your application but monitor the state of them in your application.