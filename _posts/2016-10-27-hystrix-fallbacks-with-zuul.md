---
published: true
layout: post
title: "Providing Hystrix Fallbacks For Routes In Zuul"
categories:
  - Cloud
  - Spring Cloud
  - Spring
---
If you are a user of [Zuul from Spring Cloud Netflix](https://github.com/spring-cloud/spring-cloud-netflix/blob/master/docs/src/main/asciidoc/spring-cloud-netflix.adoc#router-and-filter-zuul), than you likely know
that all the routes you configure that use Ribbon are also wrapped in Hystrix
commands.  This nice little feature provides a circuit breaker for all
your proxied requests through Zuul.  When the circuit is tripped it can save
the proxied service from becoming too overloaded and perhaps also save your app
from appearing to be "slow".  

If you have ever looked at [Hystrix](https://github.com/spring-cloud/spring-cloud-netflix/blob/master/docs/src/main/asciidoc/spring-cloud-netflix.adoc#circuit-breaker-hystrix-clients) before you know that you can generally
provide a fallback for the circuit.  The fallback provides some default functionality
for the circuit when the circuit is tripped.  For example, if you wrap a request
to a service in a Hystrix Command and provide a fallback, than when the circuit
is tripped and a request is made to the service the fallback will be invoked instead.
What would the fallback return?  One logical response
would be to return back a cached response.  This combination of circuit breaker +
fallback allows for the downstream service to recover while still providing a somewhat
meaningful response back to the calling service.

Up until now it was not possible to provide a fallback when a circuit was tripped
for a given route in Zuul.  The latest [Camden snapshot builds](http://projects.spring.io/spring-cloud/) now include a feature allowing
you to specify fallbacks for your routes configured in Zuul.  (All releases prior
to, and including `Camden.SR1` do NOT include this functionality.)  It is very simple
to provide a fallback for a route in Zuul.  All you have to do is provide a bean
that implements `ZuulFallbackProvider` in your app.  Within that bean you specify
the route ID you want the fallback to apply to and provide a `ClientHttpResponse`
object to return when the fallback is invoked.  For more information see the
[Spring Cloud docs](https://github.com/spring-cloud/spring-cloud-netflix/blob/master/docs/src/main/asciidoc/spring-cloud-netflix.adoc#providing-hystrix-fallbacks-for-routes).  If you would like to see a working example check out the
[Zuul Server](https://github.com/spring-cloud-samples/zuul-server) sample on GitHub.
