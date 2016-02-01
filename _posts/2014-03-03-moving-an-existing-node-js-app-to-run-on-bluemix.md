---
id: 498
title: Moving An Existing Node.js App To Run On BlueMix
date: 2014-03-03T09:11:35+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=498
permalink: /2014/03/03/moving-an-existing-node-js-app-to-run-on-bluemix/
dsq_thread_id:
  - 4535001765
categories:
  - BlueMix
  - Cloud
  - PaaS
---
Over the past few weeks I have been teaching myself about Node.js.  One of the tutorials I followed was a step by step guide to getting up and running with <a href="http://cwbuecheler.com/web/tutorials/2013/node-express-mongo/" target="_blank">Node.js, Express, Jade, and MongoDB</a>.  After I finished the tutorial and had the app running locally I wanted to get it deployed in the cloud on BlueMix, so I began to investigate what I had to modify in order to do this.  After reviewing the code one thing jumped out to me the I knew immediately would be a problem.  Want to take a guess at what that is?  No?  OK, it is the URL to the Mongo database.  After completing the tutorial you are left with an app that had a hardcoded DB URL.  Obviously deploying the code with the hardcoded URL was not going to work.  First before I go into what I had to change, I think a little background is necessary.

When you use a database service in BlueMix, or any Cloud Foundry based PaaS service, the information (credentials, URL, etc) about the database service is passed to the app in the VCAP_SERVICES environment variable.  You can read more about services in the <a href="https://www.ng.bluemix.net/docs/#services/services.html" target="_blank">BlueMix documentation</a>.  The Cloud Foundry documentation also has some good <a href="http://docs.cloudfoundry.com/docs/running/architecture/services/" target="_blank">documentation</a> on services.

In the following steps we will be leveraging the Cloud Foundry command line interface (CLI) to deploy our application to BlueMix.  You can learn more about the CLI in the documentation <a href="https://github.com/cloudfoundry/cli" target="_blank">here</a>.

OK back to the modifications&#8230;

I wanted to make sure I could run the app both locally and have it work in the cloud so I figured a simple if statement would do.  Further more the if statement should be able to be based on the presence of the VCAP_SERVICES environment variable.  At the top of my app.js file I added a simple if statement and some logging&#8230;.

<pre>console.log('VCAP SERVICES: ' + JSON.stringify(process.env.VCAP_SERVICES, null, 4));
var mongoUrl;
if(process && process.env && process.env.VCAP_SERVICES) {
  var vcapServices = JSON.parse(process.env.VCAP_SERVICES);
  for (var svcName in vcapServices) {
    if (svcName.match(/^mongo.*/)) {
      mongoUrl = vcapServices[svcName][0].credentials.uri;
      mongoUrl = mongoUrl || vcapServices[svcName][0].credentials.url;
      break;
    }
  }
} else {
  mongoUrl = "localhost:27017/nodetest1";
}
console.log('Mongo URL: ' + mongoUrl);</pre>

_ I would like to thank my colleague Pat Mueller for the snippet of code to extract the Mongo DB URL from VCAP_SERVICES.  Thanks Pat!_

After that you just need to tell monk to use the monogUrl variable.

<pre>var db = monk(mongoUrl);</pre>

Thats it from a coding perspective, now we need to create an instance of the Mongo DB to use in BlueMix.  We can use the cf command line tool for that but first we need to login to BlueMix.

<pre>$ cf api https://api.ng.bluemix.net
Setting api endpoint to https://api.ng.bluemix.net...
OK

API endpoint: https://api.ng.bluemix.net (API version: 2.0.0)
Not logged in. Use 'cf login' to log in.
No org or space targeted, use 'cf target -o ORG -s SPACE'

$ cf login
API endpoint: https://api.ng.bluemix.net

Username> ibmid

Password> 
Authenticating...
OK

Targeted org rjbaxter@us.ibm.com

Targeted space dev

API endpoint: https://api.ng.bluemix.net (API version: 2.0.0)
User:         ibmid
Org:          ibmid
Space:        dev</pre>

OK, now we are logged in, lets create an instance of the Mongo DB service in BlueMix to use with our app. You can learn more about the Mongo DB service from the <a href="https://www.ng.bluemix.net/docs/Services/MongoDB/Index.html" target="_blank">documentation</a>.

<pre>$ cf create-service mongodb 100 mongodbnode
Creating service mongodbnode in org ibmid / space dev as ibmid...
OK</pre>

The last thing we need to do is create a manifest.yml file in the root directory of our Node JS project that contains deployment information that BlueMix will use to deploy our app in the cloud.  In the same folder as your app.js file create a new file called manifest.yml.  Inside the manifest.yml copy this code.  Then save the file.

<pre>applications:
- name: nodetest1
  memory: 128M
  command: node app.js
  services:
  - mongodbnode</pre>

Notice the manifest.yml file is telling BlueMix the name to use for the app, how much memory we need, the command to launch the app, and the name of the Mongo DB service to bind to (the same name we gave the create-service command above).

Now we can deploy the app to BlueMix using the cf push command.  Just cd into the directory containing the manifest.yml file and call cf push.

<pre>$ cf push
Using manifest file /Users/ryanjbaxter/node-examples/nodetest1/manifest.yml

Updating app nodetest1 in org ibmid / space dev as ibmid...
OK

Uploading nodetest1...
Uploading from: /Users/ryanjbaxter/node-examples/nodetest1
11.9K, 12 files
OK
Binding service mongodbnode to nodetest1 in org ibmid / space dev as ibmid
OK

Starting app nodetest1 in org ibmid / space dev as ibmid...
-----&gt; Downloaded app package (8.0K)
OK
-----&gt; ibm-buildpack-nodejs 2013-01-16
-----&gt; Resolving engine versions
       WARNING: No version of Node.js specified in package.json, see:
       https://devcenter.heroku.com/articles/nodejs-versions
       Using Node.js version: 0.10.21
       Using npm version: 1.2.30
-----&gt; Fetching IBM SDK for Node.js binaries
-----&gt; Vendoring node into slug
-----&gt; Installing dependencies with npm
...
       Dependencies installed
-----&gt; Building runtime environment
+ '[' -e /tmp/staged/app/ACE_EMPTY_RUNTIME ']'
+ echo no
+ exit 1
-----&gt; Uploading droplet (34M)

1 of 1 instances running

App started

Showing health and status for app nodetest1 in org ibmid / space dev as ibmid...
OK

requested state: started
instances: 1/1
usage: 128M x 1 instances
urls: nodetest1.mybluemix.net

     state     since                    cpu    memory          disk          
#0   running   2014-02-23 04:19:40 PM   0.0%   19.3M of 128M   93.5M of 1G</pre>

Then if you go to http://nodetest1.mybluemix.net/newuser you can create a new user which gets persisted to the Mongo DB you tied to the app.  Thats it, you just deployed the app to the cloud!  Pretty easy!  Notice you didn&#8217;t need to worry about spinning up a VM, setting up Node, or setting up Mongo, all that is done for you, and in a very short period of time.  All that time you spent at the beginning of the tutorial setting up Node and Mongo on your local machine was saved when you went to the cloud.

Now for more complex apps there may be more changes you need to make in order to move the app to the cloud.  For example, keeping state in the app is generally a bad idea in the cloud if you have multiple instances of the app running.  I will address this concern in an upcoming blog post.  For now, happy coding!