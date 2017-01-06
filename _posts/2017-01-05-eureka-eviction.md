---
published: true
layout: post
title: "Evicting Instances From Eureka"
categories:
  - Cloud
  - Spring Cloud
  - Spring
---

Typically when you are using Spring Cloud Eureka for service discovery and you shut
down an instance that is registered with the server that instance is removed from
the registry.  However in some cases that might not be happen and instead the instance
is just marked as being `DOWN`.  If you look at your Eureka dashboard you will see
something like this.

![Eureka Dashboard](https://cloud.githubusercontent.com/assets/524254/18324140/dfc2f822-7508-11e6-85a8-090bc559d722.png)

Notice one of the `RACES` services is marked as down and the big red text above the table.
You might run into this because by default if 85% of instances in Eureka are not sending
heartbeats to the Eureka server than the server will not evict them.  When you see this,
Eureka has entered what is called "self preservation mode".  The good news is you
should not be alarmed by this.  It is actually Eureka trying to be helpful and by not evicting
instances that are not actually down.  For example, there could be some network issues
preventing instances from sending heartbeat information.  Everything is still functioning
correctly, so do you really want Eureka to start evicting instances?  Probably not.
With any luck the network issues will resolve themselves and heartbeat information will
eventually be received by the server.

If for whatever reason you want to turn off self preservation mode you can set
`eureka.server.enableSelfPreservation` to `false`.  For a more in depth discussion
on Eureka's self preservation mode check out [this question](http://stackoverflow.com/questions/33921557/understanding-spring-cloud-eureka-server-self-preservation-and-renew-threshold)
on Stack Overflow.
