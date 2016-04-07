---
published: true
layout: post
title: "Docker Compose and IBM Containers"
categories:
  - Cloud
  - Bluemix
  - Docker
---

Recently my colleague Chris Rosen published [this blog post](https://developer.ibm.com/bluemix/2016/03/24/new-deployment-architecture-for-containers/?linkId=22660520) about some new functionality
coming to the IBM Containers service in Bluemix.  One of the new features that is
now available is native [Docker Compose support](https://console.ng.bluemix.net/docs/containers/container_creating_ov.html#container_compose_ov).  Docker Compose is a great tool to use if your application is split up into multiple containers.
It takes care of a lot of the orchestration and configuration needed to be done between
the various containers so that they can work seamlessly together.  You can read more about Docker Compose in the Docker
[documentation](https://docs.docker.com/compose/).

As a demonstration of how to use Docker Compose with Bluemix I have gone ahead
and added some [new instructions](https://github.com/IBM-Bluemix/bluechatter#deploy-to-bluemix-with-docker-compose)
to the BlueChatter sample app README on how to deploy BlueChatter and the
required Redis service using Docker Compose.  In just a few steps you can have
both the BlueChatter web app and the Redis service deployed to Bluemix in their
own containers.  Below is a video demonstrating how easy it is to do this, enjoy!

<iframe width="1024" height="768" src="https://www.youtube.com/embed/igY-rA40yfY" frameborder="0" allowfullscreen></iframe>
