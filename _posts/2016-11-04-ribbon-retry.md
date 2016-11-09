---
published: true
layout: post
title: "Retrying Requests With Spring Cloud Ribbon"
categories:
  - Cloud
  - Spring Cloud
  - Spring
---

Most applications today make HTTP requests to external services.  When using
Spring Cloud one way to make these requests is to use what we call a
"[load balanced `RestTemplate`](http://cloud.spring.io/spring-cloud-static/spring-cloud.html#_spring_resttemplate_as_a_load_balancer_client)".
Creating a load balanced `RestTemplate` is pretty strait forward.

```
@Configuration
public class MyConfiguration {

    @LoadBalanced
    @Bean
    RestTemplate restTemplate() {
        return new RestTemplate();
    }
}

public class MyClass {
    @Autowired
    private RestTemplate restTemplate;

    public String doOtherStuff() {
        String results = restTemplate.getForObject("http://stores/stores", String.class);
        return results;
    }
}
```

The added `@LoadBalanced` annotation allows you to take advantage of services registered
with a discovery service, such as Eureka.  As you can see in the `doOtherStuff` method
we are making a request to the URL `http://stores/stores`.  The hostname, `stores`,
is not actually a registered hostname, instead it is the name of the stores service
registered with the discovery server.  The `RestTemplate` in this case will replace
the `stores` hostname with the host name (or IP address) of the stores service.

As we are all aware of, making requests like this can be problematic.  For any number
of reasons something may go wrong and the request will fail.  That is why, in robust
applications, requests like these may be retried when failures are encountered.  It might
be the case that the service is completely down and we are never going to get a response.
However it is equally as likely that the failure was due to some kind of fluke network issue
and a subsequent request may succeed.  Or it could be the case that the given instance of
the service experiencing problems but there is another instance available that might be
perfectly capable of handing our request.

In Spring Cloud Brixton, load balanced `RestTemplate`s would use a `RestClient` provided
by Netflix's Ribbon library.  This `RestClient` has logic built into it to retry
failed requests, however, Netflix recently deprecated the `RestClient` so
we needed to come up with another solution.  The initial HTTP client we used to replace
the Netflix `RestClient` did not have any retry logic built into it.  If your request failed
it was up to you, the developer, to retry the request.  As of `Camden.SR2` we have reintroduced
some retry handling into load balanced `RestTemplates`.  We now take advantage of the awesome
[Spring Retry project](https://github.com/spring-projects/spring-retry) to provide the retry
logic.  The good news is you can use the same [Ribbon properties as before](http://cloud.spring.io/spring-cloud-static/Camden.SR2/#_retrying_failed_requests)
to configure how many retry requests are made, and which requests are retried.

In the future we will be using Spring Retry when making API requests using Feign as well
as when requests are made through Zuul.

Enjoy!
