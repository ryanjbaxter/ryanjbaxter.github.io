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

As we are all aware, making requests like this can be problematic.  For any number
of reasons something may go wrong and the request could fail.  That is why, in robust
applications, API requests like the one above may be retried when failures are encountered.  It might
be the case that the service is completely down and we are never going to get a response.
However it is equally as likely that the failure was due to some kind of fluke network issue
and a subsequent request may succeed.  It could even be the case that a given instance of
a service may be experiencing problems but there is another instance available that might be
perfectly capable of handing our request.

In Spring Cloud, if you used a load balanced `RestTemplate` to make your API request and the request
failed it was up to you, the developer, to retry the request.  As of `Camden.SR2` we have introduced
some retry handling into load balanced `RestTemplates`.  We now take advantage of the awesome
[Spring Retry project](https://github.com/spring-projects/spring-retry) to provide the retry
logic.  You can use [Ribbon properties](http://cloud.spring.io/spring-cloud-static/Camden.SR2/#_retrying_failed_requests)
to configure how many retry requests are made, and which requests are retried.

In the future we will be using Spring Retry when making API requests with Feign as well
as when requests are made through Zuul.

Enjoy!
