---
id: 1152
title: 'Building Cloud Native Apps With Spring &#8211; Part 4'
date: 2015-10-01T13:14:26+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1152
permalink: /2015/10/01/building-cloud-native-apps-with-spring-part-4/
dsq_thread_id:
  - 4535002058
categories:
  - Cloud
  - Cloud Foundry
  - PaaS
---
After completing parts <a href="http://ryanjbaxter.com/2015/09/14/building-cloud-native-apps-with-spring-part-1/" target="_blank">1</a>, <a href="http://ryanjbaxter.com/2015/09/21/building-cloud-native-apps-with-spring-part-2/" target="_blank">2</a>, and <a href="http://ryanjbaxter.com/2015/09/29/building-cloud-native-apps-with-spring-part-3/" target="_blank">3</a> of this tutorial, we have a basic microservices application setup and running.  It is simple, but incorporates many of the mandates of being a [cloud native application](http://ryanjbaxter.com/2015/07/13/building-cloud-native-applications/), so we are off to a good start.

There are a couple of additional features in <a href="http://cloud.spring.io/spring-cloud-netflix/" target="_blank">Spring Cloud Netflix</a> that are particularly useful and worth demonstrating.  In this blog post I thought we would take a look at <a href="https://github.com/Netflix/feign" target="_blank">Feign</a>.  Rather than describe it myself, the <a href="http://cloud.spring.io/spring-cloud-netflix/spring-cloud-netflix.html#spring-cloud-feign" target="_blank">Spring Cloud documentation</a> does a good job

> [Feign](https://github.com/Netflix/feign) is a declarative web service client. It makes writing web service clients easier. To use Feign create an interface and annotate it. It has pluggable annotation support including Feign annotations and JAX-RS annotations. Feign also supports pluggable encoders and decoders. Spring Cloud adds support for Spring MVC annotations and for using the same _**HttpMessageConverters**_ used by default in Spring Web. Spring Cloud integrates Ribbon and Eureka to provide a load balanced http client when using Feign.

In summary, if you are looking to build a REST API client, Feign is your friend.  Making REST API calls from one service to another is a very common pattern in microservice applications so this functionality will prove particularly useful.  Also notice that Spring Cloud adds support for Ribbon and Eureka to Feign.  We are already familiar with what Eureka does from the <a href="http://ryanjbaxter.com/2015/09/21/building-cloud-native-apps-with-spring-part-2/" target="_blank">previous parts of this tutorial</a>, but what is Ribbon?

<a href="http://cloud.spring.io/spring-cloud-netflix/spring-cloud-netflix.html#spring-cloud-ribbon" target="_blank">Ribbon</a> is another project from Netflix OSS and is what amounts to a client side load balancer.  This is important for obvious reasons when we start to look at services distributed over different data centers and geographies.  For example, Ribbon will use Eureka to figure out which instance of a service it should make a request to.  We will talk about Ribbon more in a future blog post, for now just know that is will load balance our requests from our Feign clients.

## Getting Started With Feign

Now that we know what Feign is and why we want to use it, lets leverage it in our sample application.  Right now in order to get the participants for races we have to make a request to _/races_ to get all the races and then make a request to _/participants/races/{id}_ in order to get the participants for a given race.  In some situations this might be OK, but maybe not for all situations.  Consider the case where our app is being used on a mobile device.  Since the network on a mobile device is typically a lot slower than a desktop we might want to limit the number of network requests we need to make in order to get all the data we need.  In other words a mobile client might want to make a single request to get all the race and participant data instead of multiple requests.  Lets introduce a new API for mobile clients to use that does just that.

Our new API will need to use both the races service and the participants service.  Since it is really returning data about races it makes sense for it to live in the races service.  We need a way to call the participants service from the races service, this is where Feign comes in.  First lets open the POM file for our races service and add the Feign dependency to our project.  In the dependencies section of your POM add the following dependency

<pre class="font-size:15 lang:default decode:true">&lt;dependency&gt;
    &lt;groupId&gt;org.springframework.cloud&lt;/groupId&gt;
    &lt;artifactId&gt;spring-cloud-starter-feign&lt;/artifactId&gt;
&lt;/dependency&gt;</pre>

Now lets open _OcrRacesApplication.java_ in _com.ryanjbaxter.spring.cloud.ocr.races_.  First add _@EnableFeignClients_ to the _OcrRacesApplication_ class.  Then create a new interface that will acts as our REST client for talking to the participants service.  Create a new class called _ParticipantsClient_ with the following code

<pre class="font-size:15 lang:default decode:true ">@FeignClient("participants")
interface ParticipantsClient {
	
	@RequestMapping(method = RequestMethod.GET, value="/races/{raceId}")
	List&lt;Participant&gt; getParticipants(@PathVariable("raceId") String raceId);
	
}</pre>

The first thing you notice is that this interface uses an annotation called _@FeignClient_.  This annotation is telling Feign that we will be talking to a service called _&#8220;participants&#8221;_.  Feign will use Eureka to figure out the correct URL for the participants service.  The rest of the interface should look pretty familiar to you if you are familiar with Spring MVC.  The _@RequestMapping_ annotation on the _getParticipants_ method is telling Feign to make a _GET_ request to the participants service at the path _/races/raceId_.

At this point you will have compile errors in your interface because there is no class called _Participant_ in the races service.  If you are a seasoned Java developer, your first instinct will probably be to do some refactoring.  You might go to the participants service extract out the _Participant_ class into its own project.  Then you would change the participants and races service so they depend on this new project.  This has been engrained in our minds due to the DRY (do not repeat yourself) principal, which says we should not be copying and pasting code all over the place due to the fact that it will become unmaintainable.  This is certainly a valid concern, however we have to balance the DRY principal along with other principals of microservices.  The problem with this approach to refactoring our application is that we now have a common class used by 2 (or more) services.  What happens when one service needs to make a change to that class?  If the change is drastic enough, you can break the other service.  This means that the services can&#8217;t evolve independently of each other, which is one of the benefits we are trying to achieve by using microservices.

At the end of the day you have to make a decision that is right for you, your team, and your project.  Do you want to share code between your microservices or do you want the benefit of being able to evolve your services independently of each other?  In this case we will NOT follow the DRY principal and create a new _Participant_ class in our races service.  Why?  Think about how you would be working if you were building a real production grade microservices application.  You would be a developer on a team that is responsible for a single service.  In theory you will know nothing about the implementations of other services you depend on, the only thing you can rely on is their public API.  They may not even be implemented in the same language that you are using.  Based on that logic, it makes sense for you to create a Participant class in your service which corresponds to what their public API will return.  In my opinion, when it comes to microservices, sharing code between services does not generally make sense.

In _OcrRacesApplication.java_ create a new _Participant_ class

<pre class="font-size:15 lang:default decode:true ">class Participant {
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
	
	public Participant(){}
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
}</pre>

Finally we will need a new _Race_ class which contains the information about the participants in the race.  Here, again we have a couple of options.  We can add a list of participants to our existing _Race_ class but doing this would result in this weird _participants_ property in the JSON that is always an empty array when we make requests to _/races_.  It seems odd to me as a consumer to have a property that seemingly never gets used until we call this new API we are creating.  The second option is create a subclass of _Race_ which contains participants.  Add a new class called _RacesWithParticipants_

<pre class="font-size:15 lang:default decode:true ">class RaceWithParticipants extends Race {
	private List&lt;Participant&gt; participants;

	public RaceWithParticipants(Race r, List&lt;Participant&gt; participants) {
		super(r.getName(), r.getId(), r.getState(), r.getCity());
		this.participants = participants;
	}

	public List&lt;Participant&gt; getParticipants() {
		return participants;
	}

	public void setParticipants(List&lt;Participant&gt; participants) {
		this.participants = participants;
	}
}</pre>

&nbsp;

## Using The Feign Client

Now we are ready to create our new API which will return race data including participant information.  In the _OcrRacesApplication_ class where we have our existing _getRaces_ API create a new API called _getRacesWithParticipants_.  But before we do that we will need an instance of _ParticipantsClient_ which we will use in the new API to call the participants service.  Add a new variable to the class

<pre class="font-size:15 lang:default decode:true ">@Autowired
private ParticipantsClient participantsClient;</pre>

Now add the new API

<pre class="font-size:15 lang:default decode:true ">@RequestMapping("/participants")
	public List&lt;RaceWithParticipants&gt; getRacesWithParticipants() {
		List&lt;RaceWithParticipants&gt; returnRaces = new ArrayList&lt;RaceWithParticipants&gt;();
		for(Race r : races) {
			returnRaces.add(new RaceWithParticipants(r, participantsClient.getParticipants(r.getId())));
		}
		return returnRaces;
	}</pre>

That is all the code we need to write, now lets test out our new API.  Start the eureka, races, participants, and web services and go to <a href="http://localhost:8080/races/participants" target="_blank">http://localhost:8080/races/participants</a>.  (Make sure you let all the services register themselves with Eureka before trying the API.)  This should return some JSON that looks like this

<pre class="font-size:15 lang:default decode:true">[
   {
      "name":"Spartan Beast",
      "id":"123",
      "state":"MA",
      "city":"Boston",
      "participants":[
         {
            "firstName":"Ryan",
            "lastName":"Baxter",
            "homeState":"MA",
            "shirtSize":"S",
            "races":[
               "123",
               "456"
            ]
         }
      ]
   },
   {
      "name":"Tough Mudder RI",
      "id":"456",
      "state":"RI",
      "city":"Providence",
      "participants":[
         {
            "firstName":"Ryan",
            "lastName":"Baxter",
            "homeState":"MA",
            "shirtSize":"S",
            "races":[
               "123",
               "456"
            ]
         },
         {
            "firstName":"Stephanie",
            "lastName":"Baxter",
            "homeState":"MA",
            "shirtSize":"S",
            "races":[
               "456"
            ]
         }
      ]
   }
]</pre>

At the same time you can also continue to use the _/races_ API at <a href="http://localhost:8080/races" target="_blank">http://localhost:8080/races</a> to just get the race data.

As you can see Feign makes it very easy to build REST clients for other services in your microservices app.  Just by using a few annotations and minimal Java code you can easily construct a REST client for any service you want.

&nbsp;