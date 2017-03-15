---
published: true
layout: post
title: "Retrying HTTP Requests In Spring Cloud Netflix"
categories:
  - Cloud
  - Spring Cloud
  - Spring
---

As I have talked about before, one of the key aspects of cloud native applications is resiliency.  In order to be robust and avoid all the potential problems that can come from running in  such a volatile environment like the cloud your application needs to be able to handle anything that can be thrown at it.  One of the more common operations a cloud native application will do is make HTTP requests to other applications.  These applications may be under your control as well or they could be third party apps, but in the end it doesn’t really matter who controls them, there is always the potential for failure when making any kind of network request.

Cloud native apps should be able to deal with any kind of failure in a graceful manor.  Circuit breakers, which I have talked about [before](http://ryanjbaxter.com/2015/10/12/building-cloud-native-apps-with-spring-part-5/), are one way of dealing with failures.  Before we get to the point of tripping a circuit breaker, we can try to be a smarter about the network requests we make.  Just because a network request fails once doesn’t mean we should give up and all hope is lost.  

Network requests can fail for a number of reasons.  Yes the service you may be making a request to may be down, which is probably the worst case scenario, but there are other reasons as well.  Maybe it is just the network itself having issues.  Maybe the service you are trying to make a request to is actually up but is temporarily unavailable.  Whatever the reason may be for the failure, it could be a short term issue.  If you were to make the same request shortly after the failure occurs it may succeed.  So in order to make our applications more resilient we can retry failed requests in the hope that subsequent requests might succeed.  

If you are familiar with Spring Cloud Netflix, there are a number ways to make HTTP requests.  The three main ways are using a [load balanced `RestTemplate`](http://cloud.spring.io/spring-cloud-static/Camden.SR5/#_spring_resttemplate_as_a_load_balancer_client), [Feign](http://cloud.spring.io/spring-cloud-static/Camden.SR5/#spring-cloud-feign), and [Zuul](http://cloud.spring.io/spring-cloud-static/Camden.SR5/#_router_and_filter_zuul).  In Spring Cloud Brixton all three of these scenarios leveraged an HTTP client from Netflix that has built-in logic to retry failed requests.  However, this HTTP client was deprecated and we needed to come up with our own HTTP client to use.  Unfortunately when implementing the new HTTP client we did not originally include logic to retry failed requests.  As of [Spring Cloud Camden.SR6](https://spring.io/blog/2017/03/10/spring-cloud-camden-sr6-is-available) all three use cases now once again have logic to retry failed requests.

To achieve this we leveraged [Spring Retry](https://github.com/spring-projects/spring-retry) as an easy way of implementing the retry logic.  Because we leverage Spring Retry, you must make sure Spring Retry is on the classpath of your application in order to enable the retry logic.  Without Spring Retry on the classpath you will find that failed requests will not be retried.

You can control how many times a request is retried using standard Ribbon properties.
To control how many times a request is retried on the same server you can adjust `sample-client.ribbon.MaxAutoRetries`.  To control how many servers to try you can adjust `sample-client.ribbon.MaxAutoRetriesNextServer`.  The total number of retries that are made will be equal to `sample-client.ribbon.MaxAutoRetries * sample-client.ribbon.MaxAutoRetriesNextServer`.  You can also control whether just `GET` requests will be retried or whether all requests will be retried by adjusting `sample-client.ribbon.OkToRetryOnAllOperations`.  The above properties apply whether you are using a load balanced `RestTemplate`, Feign, or Zuul.

When using Zuul there are some additional properties you can configure to control retry functionality.  Setting `zuul.retryable` to `true` or `false` will control whether all or none of the proxied requests will be retried.  By default it is set to `true`.  You can also control retry functionality on a route by route basis by setting `zuul.routes.routename.retryable`.

If you have been sticking to using the deprecated Netflix `RestClient` due to the lack of retry logic in the newer HTTP clients hopefully this will now allow you to switch to using the newer clients.  The retry logic we have added should work for both the Apache based HTTP clients of the OK HTTP clients, so you have your choice as to which one you want to use.
