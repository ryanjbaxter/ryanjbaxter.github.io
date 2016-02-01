---
id: 1229
title: 'Building Cloud Native Applications With Spring &#8211; Part 8'
date: 2015-12-15T16:36:25+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1229
permalink: /2015/12/15/building-cloud-native-applications-with-spring-part-8/
dsq_thread_id:
  - 4535002076
categories:
  - BlueMix
  - Cloud
  - Docker
---
In my [previous blog post](http://ryanjbaxter.com/2015/11/04/building-cloud-native-apps-with-spring-part-7/) I explained how to deploy a Spring Cloud based cloud native app to Bluemix.  However, you may want to deploy our cloud native app to other environments as well.  For example, a testing environment that is inside your own data center.  Or maybe you want to have the ability for developers working on another microservice to easily spin up all the other microservices of the cloud native app so they can easily integrate their service into the mix.  One solution to these problems is to use a <a href="https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=4&cad=rja&uact=8&ved=0ahUKEwjXgdrV2tTJAhUCWz4KHYPJAkIQFgg2MAM&url=https%3A%2F%2Fen.wikipedia.org%2Fwiki%2FLXC&usg=AFQjCNGtZ1byPFO5VYfmcGM7OX9FeToIig&sig2=1ke8G9tgApm8kNQEz80DBg&bvm=bv.109910813,d.cWw" target="_blank">Linux container</a> technology called <a href="https://www.docker.com/" target="_blank">Docker</a>.

<img class="alignnone size-full wp-image-1236" src="http://ryanjbaxter.com/wp-content/uploads/2015/12/docker-wallpaper-black.jpg" alt="docker-wallpaper-black" width="2880" height="1800" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/12/docker-wallpaper-black-300x188.jpg 300w, http://ryanjbaxter.com/wp-content/uploads/2015/12/docker-wallpaper-black-768x480.jpg 768w, http://ryanjbaxter.com/wp-content/uploads/2015/12/docker-wallpaper-black-1024x640.jpg 1024w, http://ryanjbaxter.com/wp-content/uploads/2015/12/docker-wallpaper-black.jpg 2880w" sizes="(max-width: 2880px) 100vw, 2880px" />

At this point I hope you have heard of Docker, I am not going to spend any time here going over what it is, besides to say it is considered the go to Linux container technology at this point.  There is a ton of material on the internet (Google is your friend) if you are unfamiliar with Docker, but I suggest you start with the official <a href="http://docker.com" target="_blank">Docker website</a>.  By using Docker we can run our cloud native application virtually anywhere, the only thing Docker requires is Linux.  Once you have a Linux machine with the Docker daemon installed you can run any Docker container, including our cloud native application.  Once our cloud native application is running in Docker containers, you won&#8217;t need to know anything about Spring, Spring Cloud, or even Java to get it up and running.  In addition, a number of cloud vendors (including <a href="https://www.ng.bluemix.net/docs/containers/container_index.html" target="_blank">Bluemix</a>) support running Docker containers on their cloud platforms so it allows you to avoid any cloud vendor lockin.

Because of the benefits listed above, and many others, it makes a lot of sense to be able to run our cloud native application inside a set of Docker containers.  Yes, that is not a typo, we want to run multiple containers, one for each microservice in fact.  There are two reasons why we want to use multiple Docker containers instead of running everything in one container.  First it allows us to continue to adhere to the cloud native principal that each service should be independent of each other.  Second, [Docker suggests that you run a single process per container](https://docs.docker.com/engine/articles/dockerfile_best-practices/).

Before we get started creating containers for the various services in our application you will need to make sure you have a working Docker environment setup to test on.  Docker can be installed on your favorite OS (Windows, OSX, and Linux).  Head over to the [Docker documentation](https://docs.docker.com/engine/userguide/) if you need to setup a Docker environment.  Once you can run a container you should be able to follow the rest of this post with ease.

## Dockerfiles

To get started we will need to create a Dockerfile for each of our services (races, participants, web, eureka, and hystrix).  There is a great post on spring.io about [how to get started with Docker and Spring Boot](https://spring.io/guides/gs/spring-boot-docker/).  Since Spring Cloud builds upon Boot, everything in this blog post applies to our microservices.  The section entitled &#8220;Containerize it&#8221; is the most relevant section for what we are trying to do with our app.  This section shows an example Dockerfile used to run a Spring Boot application.  We can copy this Dockerfile and modify it slightly for our microservices.  Lets start by adding a Dockerfile for our Eureka server.

In the root of the Eureka service project (the directory containing the pom.xml file) create a new file with the name Dockerfile.  Add the following content to the file.

<pre class="font-size:15 lang:default highlight:0 decode:true">FROM java:8
VOLUME /tmp
ADD target/ocr-eureka*.jar app.jar
RUN bash -c 'touch /app.jar'
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]</pre>

If you want to understand more about the various commands in this Dockerfile take a look at the <a href="http://docs.docker.com/engine/reference/builder/" target="_blank">Dockerfile reference</a>.

The most important part of the file is the ADD command which adds the jar for our application to the container.  With this simple Dockerfile we should now be able to run our Eureka server in a container.  You can test this out by opening a terminal window and navigating to the directory containing the Dockerfile.  Then run

<pre class="lang:default highlight:0 decode:true">$ docker build -t ocr/eureka ./
$ docker run -i ocr/eureka</pre>

The _docker build_ command will build a Docker image based on your Dockerfile and tag it in your local registry with _ocr/eureka_.  (After the build finishes you can confirm the image is in your local registry by running_ docker images_ from your command line.)  The _docker run_ command then starts the image we just built (the -i parameter does this interactively so you can see the application logging).  You can then confirm your Eureka server is running by going to _http://<dockerhost>:8761_.  You will need to replace _dockerhost_ in the previous URL with the host/ip address of your Docker daemon.  If you are on OSX or Windows this will most likely be a VM of some kind.  If you are Linux than it may be just be localhost.

Now that we have one service running in a Docker container, containerizing the rest of the services should be easy.  The Dockerfile we used for Eureka can pretty much be copied exactly as is for the other services.  The only change you will need to make is in the ADD command.  You will need to change the ADD command to use the correct name for the jar of the service.  For example the ADD command in the Dockerfile for the races service would look like

<pre class="font-size:15 lang:default highlight:0 decode:true">ADD target/ocr-races*.jar app.jar</pre>

Using the Eureka Dockerfile as an example go ahead and create Dockerfiles for the remaining services (races, participants, web, and hystrix).

## Linking Containers

If you try starting the other services in their containers you may notice a problem, they can&#8217;t register with Eureka.  Each service is looking for Eureka at localhost:8761, but Eureka is running in a completely separate container so how do the services talk to each other?  A more general question would be how do you link together multiple Docker containers?  The _docker run_ command does have a <a href="https://docs.docker.com/v1.8/userguide/dockerlinks/" target="_blank"><em>&#8211;link</em> parameter</a> where you can specify the name or id of another container to link to.  This will setup some networking between the containers and allow them to communicate with each other.  However, with the number of containers we have for our cloud native application this approach is a little overwhelming.  A better option is to use another tool from Docker called <a href="https://docs.docker.com/compose/" target="_blank">Docker Compose</a>.  Docker Compose is a simple tool for orchestrating running multiple containers.

### Docker Compose For The Win

To use Docker Compose we need to create a new file called _docker-compose.yml_.  This YAML file will tell Docker Compose which containers we want to start, how we want to start them, and how to link them together.  I find it easiest to place the _docker-compose.yml_ file in the root of the folder containing all the other project directories in a folder called _docker_.  For example, the directory structure for the OCR application looks like this on my machine

<pre class="font-size:15 lang:default highlight:0 decode:true">├── ocr-races
│   ├── Dockerfile
│   ├── pom.xml
│   ├── src
│   └── target
├── docker
│   └── docker-compose.yml
├── ocr-eureka
│   ├── Dockerfile
│   ├── pom.xml
│   ├── src
│   └── target
├── ocr-hystrix-dashboard
│   ├── Dockerfile
│   ├── mvnw
│   ├── mvnw.cmd
│   ├── pom.xml
│   ├── src
│   └── target
├── ocr-participants
│   ├── Dockerfile
│   ├── pom.xml
│   ├── src
│   └── target
└── ocr-web
    ├── Dockerfile
    ├── pom.xml
    ├── src
    └── target
</pre>

Notice I have a directory called _docker_ at the same level as the folders for the other services.  Inside that directory is my _docker-compose.yml_ file.  I suggest you setup your projects in the same manor, however you don&#8217;t have to.  If you don&#8217;t use the same directory structure than the following _docker-compose.yml_ file will need to be changed to match however you have chosen to lay out your projects.

Below is my _docker-compose.yml_ file.

<pre class="font-size:15 lang:default decode:true">web:
  build: ../ocr-web
  ports:
    - "8080:8080"
  environment:
    - SPRING_PROFILES_ACTIVE=docker
  links:
    - eureka
    - participants
    - races

races:
  build: ../OCR-Races
  ports:
    - "8282:8282"
  environment:
    - SPRING_PROFILES_ACTIVE=docker
  links:
    - eureka
    - participants

participants:
  build: ../ocr-participants
  ports:
    - "8181:8181"
  environment:
    - SPRING_PROFILES_ACTIVE=docker
  links:
    - eureka

hystrix:
  build: ../ocr-hystrix-dashboard
  ports:
    - "8383:8383"
  environment:
    - SPRING_PROFILES_ACTIVE=docker
  links:
    - eureka
    - races
    - participants
    - web

eureka:
  build: ../ocr-eureka
  ports:
    - "8761:8761"</pre>

Let me explain a little about this file first.

The top level properties (in this case eureka, web, races, participants, and hystrix) define individual container names.  Each one is a separate container that Docker Compose will manage for us.  Underneath these top level properties you will see each container has a build property.  This property specifies the directory of the Dockerfile for the container (this is why the directory structure is important).  Next we have a ports property which exposes ports on each container.  In this case we are exposing the HTTP port that our Spring Boot app is configured to use.  We also have a links property.  This specifies which containers you want to link together.  For example, our Eureka container does not need to communicate with  any of the other containers so there is no links property for that container.  On the opposite end of the spectrum, every other container uses Eureka so they all link to it.  The link property also does some magic for us inside the container.  When you link one container to another in a _docker-compose.yml_ file, Docker Compose will automatically create an entry in the container&#8217;s hosts file mapping the internal container IP address to the Docker container name.  For example, inside our web container the hosts file will contain an entry for the eureka, participants, and races services.  That means if our web container needs to make an HTTP request to one of these services, say Eureka, it would use a hostname of eureka to make the request.  Pretty neat right?  This makes communication between containers much easier.  You can read more about docker-compose.yml files in the <a href="https://docs.docker.com/compose/compose-file/" target="_blank">reference documentation</a>.

### Spring Profiles For Docker

Even with Docker Compose in place we still have another problem with running all these containers, and it has to do with Eureka.  Each service needs to communicate with the Eureka server.  To find out where Eureka is running we have configured each service to do one of two things.  If there is an environment variable called VCAP\_SERVICES present than the service will use that environment variable to look for the Eureka server URL.  If VCAP\_SERVICES is not avaialable than the service assumes Eureka is running on localhost:8761.  When we start these services in a Docker container there is no VCAP_SERVICES environment variable present so each service is going to assume Eureka is running on localhost:8761 and this is obviously not right.  So how do we solve this problem?

There are probably multiple solutions to the problem, but I chose to solve it by creating another Spring Boot profile for the services to use when running in a Docker container (we also did this to deal with a similar problem when running the services in Bluemix/Cloud Foundry).  You may have noticed the many of the containers in my docker-compose.yml file above have the SPRING\_PROFILES\_ACTIVE environment variable set to _docker_.  This is activating the docker profile in these applications when they are running inside a container.  Lets take a look at my _application.yml_ file for the races service as an example.

<pre class="font-size:15 lang:default highlight:0 decode:true">server:
  port: 8282
  
eureka:
  client:
    serviceUrl:
      defaultZone: ${vcap.services.ocr-eureka.credentials.uri:http://localhost:8761/eureka/}
  instance:
    hostname: ${vcap.application.uris[0]:localhost}
    metadataMap:
      instanceId: ${vcap.application.instance_id:${spring.application.name}:${spring.application.instance_id:${server.port}}}
      
---
spring:
  profiles: cloud
eureka:
  instance:
    nonSecurePort: 80
    
---
spring:
  profiles: docker
eureka:
  client:
    serviceUrl:
      defaultZone: http://${EUREKA_PORT_8761_TCP_ADDR}:8761/eureka/
  instance:
    preferIpAddress: true</pre>

Here you can see I have added a new profile called docker.  This profile overides some of the Eureka configuration.  First, we override the location of the Eureka server using the _defaultZone_ property.  You will notice the URL is using an environment variable called EUREKA\_PORT\_8761\_TCP\_ADDR.  Where does this environment variable come from?  It actually comes from <a href="https://docs.docker.com/compose/env/" target="_blank">Docker Compose</a>.  In addition to creating entries in the container&#8217;s host file for all linked containers, Docker Compose also creates environment variables containing things like the linked container&#8217;s IP address.  Since we can do environment variable substitution in our _application.yml_ file, I thought it would be convenient to use this environment variable to point to the location of the Eureka server when running in a Docker container.  There is some slight fragility in this approach in that if we change the name of the Eureka container in our Docker Compose file this would in turn change the name of the environment variable and this configuration would break.  However, for this simple example this approach works just fine.

You will also notice that in the Docker profile we are setting _preferIpAddress_ to _true_.  Without this configuration the races service would register a hostname with Eureka based on what java.net.InetAddress returns.  In the Docker environment other services will have no idea what that hostname is, so we need to make sure that when other services ask Eureka where the races service is located that we use the IP address Docker has assigned to container.  Another solution to this problem would be to set the Eureka host property to the hostname we have configured for the container in our Docker Compose file.  If we did this however we would introduce the same fragility we already have above with configuring the Eureka server.

This same profile can be added to the _application.yml_ file of all the other services that need to communicate with Eureka (hystrix, participants, and web).

## Test It Out!

Now that we have these additional profiles we can actually test things out using Docker Compose.  In your terminal window navigate to the directory containing your _docker-compose.yml_ file.  Then run the following command

<pre class="font-size:15 lang:default highlight:0 decode:true ">$ docker-compose build</pre>

This will build images for all the containers in _docker-compose.yml_ file (if needed).  Then run

<pre class="font-size:15 lang:default highlight:0 decode:true ">$ docker-compose up</pre>

This will start all the containers defined in the _docker-compose.yml_ file.  You will have to wait for all the containers to start and to register with Eureka before you test everything out.  Once everything has started go to http://<dockerhost>:8080.  The application should work as normal.  You should also be able to use Hystrix and Turbine with some slight changes.  You can get to the Hystrix dashboard by going to http://<dockerhost>:8383/hystrix.  If you want to look at the circuit breakers for the web service you need to point Hystrix to the web container.  Remember from the perspective of the container running Hystrix, the web container can be accessed using the hostname web.  To do this, the stream URL you need to enter to monitor those circuit breakers is http://web:8080/hystrix.stream.  Your initial thought might be to enter http://<dockerhost>:8080/hystrix.stream, but the Hystrix container is the one making the request to the web container, and it has no idea what <dockerhost> is, but it does know that the web container can be accessed using the hostname web.  The same is true for the races service, to view the circuit breakers you would enter the URL http://races:8282/hystrix.stream.  For Turbine you would use http://localhost:8383/turbine.stream?cluster=RACES for races or http://localhost:8383/turbine.stream?cluster=WEB for web.  Again we are using the URL localhost because Turbine is running inside the Hystrix container so it just needs to talk to itself.

Our cloud native application is now running inside a set of Docker containers making our application much more portable than before.  The next step you might want to take here is deploy images for all the containers to some type of Docker Registry, whether that is DockerHub or a private registry (see the <a href="https://docs.docker.com/compose/compose-file/#image" target="_blank">image</a> property in the Docker Compose reference documentation).  Once the images are in the registry you can change you Docker Compose file to pull these images from the registry as opposed to building them locally.  This again makes the application extremely portable because all you need to run the application is the Docker Compose file.