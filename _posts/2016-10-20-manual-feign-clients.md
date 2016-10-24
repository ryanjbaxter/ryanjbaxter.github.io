---
published: true
layout: post
title: "Manual Creation Of Feign Clients In Spring Cloud"
categories:
  - Cloud
  - Spring Cloud
  - Spring
---

![Saying](/wp-content/uploads/2016/10/saying.png)

Have you ever heard the saying above?  Sometimes it is true even when writing code.  
While Spring Cloud makes it extremely easy to [create Feign Clients](https://github.com/spring-cloud/spring-cloud-netflix/blob/master/docs/src/main/asciidoc/spring-cloud-netflix.adoc#how-to-include-feign)
with its use of annotations,
sometimes you just have to bite the bullet and create them yourselves.  I would
say the 90% of the time, the combination of annotations and
[custom configuration](https://github.com/spring-cloud/spring-cloud-netflix/blob/master/docs/src/main/asciidoc/spring-cloud-netflix.adoc#overriding-feign-defaults)
will be good enough for most use cases.  Sometimes however, you run into use
cases where that just won't do.  In these cases it would be good to have full
control over how you create your Feign clients.

Recently I added some [documentation](https://github.com/spring-cloud/spring-cloud-netflix/blob/master/docs/src/main/asciidoc/spring-cloud-netflix.adoc#creating-feign-clients-manually)
to the Spring Cloud docs that demonstrates
exactly how you would do this.  Essentially you use the same APIs as you would
if you were just using [vanilla Feign](https://github.com/OpenFeign/feign#basics).
Below is the example taken from the Spring Cloud docs.

~~~java
@Import(FeignClientsConfiguration.class)
class FooController {

	private FooClient fooClient;

	private FooClient adminClient;

    @Autowired
	public FooController(
			ResponseEntityDecoder decoder, SpringEncoder encoder, Client client) {
		this.fooClient = Feign.builder().client(client)
				.encoder(encoder)
				.decoder(decoder)
				.requestInterceptor(new BasicAuthRequestInterceptor("user", "user"))
				.target(FooClient.class, "http://PROD-SVC");
		this.adminClient = Feign.builder().client(client)
				.encoder(encoder)
				.decoder(decoder)
				.requestInterceptor(new BasicAuthRequestInterceptor("admin", "admin"))
				.target(FooClient.class, "http://PROD-SVC");
    }
}
~~~

In this example we are creating two Feign Clients from the same interface,
`FooClient.class`, which are using two different request interceptors.  Doing
Doing this with the `@FeignClient` annotation plus configuration would not be possible,
so in this case we need to create the clients using the Feign APIs.

For a more in depth look at the code, check out
[this GitHub repo](https://github.com/ryanjbaxter/manual-feign-demo).
