---
id: 672
title: Pushing A Java App To BlueMix
date: 2014-03-24T13:00:45+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=672
permalink: /2014/03/24/pushing-a-java-app-to-bluemix/
dsq_thread_id:
  - 4535001791
categories:
  - BlueMix
  - Cloud
  - PaaS
---
In my [previous video](http://ryanjbaxter.com/2014/03/12/an-introduction-to-the-bluemix-ui/ "An Introduction To The BlueMix UI") I took you through a tour of the BlueMix UI, but we didn&#8217;t actually go through pushing an application up to BlueMix and seeing the results.  Well no fear, I have created videos for that as well.  I actually split this up into two videos.  In the first one we create a Java app in the BlueMix UI, download the code for the app, make some changes, and then push it back to BlueMix.  In part two we learn about scaling our Java app and how to take advantage of the services in BlueMix to deal with the elasticity of the cloud.  Below are the videos AND the code I wrote during the demonstration.

**Part 1**

<span class="youtube"></span>

Here is the code.

<pre>package com.ibm.cloudoe.samples;

import javax.ws.rs.GET;
import javax.ws.rs.Path;

@Path("/counter")
public class Counter {

	private static int counter = 0;

	@GET
	public String getCounter() {
		counter++;
		return "This servlet was called " + counter + " times.";
	}
}</pre>

&nbsp;

**Part 2**

<span class="youtube"></span>

Here is the code we wrote in part 2.

<pre>package com.ibm.cloudoe.samples;

import java.util.Iterator;

import javax.ws.rs.GET;
import javax.ws.rs.Path;

import org.apache.wink.json4j.JSONArray;
import org.apache.wink.json4j.JSONException;
import org.apache.wink.json4j.JSONObject;

import redis.clients.jedis.Jedis;
import redis.clients.jedis.JedisPool;
import redis.clients.jedis.JedisPoolConfig;
import redis.clients.jedis.Protocol;

@Path("/counter")
public class Counter {

	private static Jedis jedis;

	public Counter() throws JSONException {
		jedis = getJedis();
	}

	@GET
	public String getCounter() {
		String value = jedis.get("counter");
		int counter = 1;
		if(value != null) {
			counter = Integer.parseInt(value);
			counter++;
		}
		jedis.set("counter", Integer.toString(counter));
		return "This servlet was called " + counter + " times.";
	}

	private Jedis getJedis() throws JSONException {
		JSONObject service = getServiceByName("redis-java-demo");
		JSONObject creds = service.getJSONObject("credentials");
		String host = creds.getString("host");
		int port = creds.getInt("port");
		String pwd = creds.getString("password");
		JedisPool pool = new JedisPool(new JedisPoolConfig(), host, port, Protocol.DEFAULT_TIMEOUT, pwd);
		return pool.getResource();
	}

	private JSONObject getServiceByName(String name) throws JSONException {
		JSONObject service = null;
		JSONObject envServices = new JSONObject(System.getenv("VCAP_SERVICES"));
		Iterator iter = envServices.keys();
		while(iter.hasNext()) {
			String key = iter.next();
			JSONArray servArray = envServices.getJSONArray(key);
			Iterator servIter = servArray.iterator();
			while(servIter.hasNext()) {
				JSONObject tempService = servIter.next();
				if(name.equals(tempService.getString("name"))) {
					service = tempService;
					break;
				}
			}
			if(service != null) {
				break;
			}
		}
		return service;
	}

}</pre>

Remeber for this code to compile you need to add the <a href="https://github.com/xetorthio/jedis/wiki/Getting-started" target="_blank">Jedis jar</a> and the <a href="http://commons.apache.org/proper/commons-pool/download_pool.cgi" target="_blank">Apache Commons Pool jar</a> (you must download version 1.6) to WebContent/WEB-INF/libs directory and modify the path element of the build.xml to include these jars.

<pre>&lt;path id="classpathDir">
        &lt;pathelement location="bin"/>
        
    	&lt;fileset dir="WebContent/WEB-INF/lib">
    		&lt;include name="**/*.jar"/>
    	&lt;/fileset>
    &lt;/path>
</pre>

&nbsp;