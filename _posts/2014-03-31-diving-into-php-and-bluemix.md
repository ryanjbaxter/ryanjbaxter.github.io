---
id: 685
title: Diving Into PHP and BlueMix
date: 2014-03-31T12:39:35+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=685
permalink: /2014/03/31/diving-into-php-and-bluemix/
categories:
  - BlueMix
  - Cloud
  - Cloud Foundry
  - PaaS
---
This past week I have been working on writing an app in PHP and getting it deployed on BlueMix.  The first thing I did before I started was take a look at the <a href="http://docs.cloudfoundry.org/" target="_blank">buildpacks</a> that were available.  Last week I made the mistake of picking one without trying it first and wasted a huge amount of time trying to figure out what was wrong with my app when it ended up being a problem with the buildpack.  This time I decided I would put in a little effort into seeing which ones worked first.  Browsing the Cloud Foundry <a href="https://github.com/cloudfoundry-community/cf-docs-contrib/wiki/Buildpacks" target="_blank">community builpacks</a> page I found three PHP buildpacks to choose from.  I created a very simple PHP app and tried all three, they all worked with the simple app so I just had to pick one.  I ended up choosing this <a href="https://github.com/dmikusa-pivotal/cf-php-build-pack" target="_blank">one</a> for a couple of reasons.

  1. It seemed to be documented fairly well.
  2. It said it had support for different extensions build in, like Mongo DB, and Redis.
  3. It had been tested against and provided a good number of samples that worked with the buildpack.

I decide it was best to first setup a local dev environment to do some local development before deploying to BlueMix.  I had never written a single line of PHP before so I was starting from scratch.  After reading about the best way to setup a PHP dev environment, I eventually decided to use <a href="http://www.mamp.info/en/" target="_blank">MAMP</a>.  I liked it because it contained an isolated PHP, Apache, and MySQL environment.  By using MAMP I didn&#8217;t have to go through all the installation and configuration of PHP and Apache on my local machine.  Just download MAMP, start it up, and your Apache and PHP environment is ready to go.  I didn&#8217;t really care about the MySQL part since I wasn&#8217;t planning on using it but it was part of the package.

My goal was to build a REST API in PHP and I had decided on using the <a href="https://github.com/codeguy/Slim" target="_blank">Slim framework</a>.  The only real reason I chose Slim is because the APIs were simple and easy to understand (I am a big fan of simplicity).  The documentation pointed out that I would need to configure some Apache rewrite rules to make things work.  The ones from the documentation worked perfectly for me locally.  My next question was how do I do this with the PHP buildpack in BlueMix?  I eventually came across this <a href="https://github.com/dmikusa-pivotal/cf-php-build-pack/issues/1" target="_blank">GitHub issue</a> which pointed me in the right direction and to a sample.  Perfect!  Just create a httpd-php.conf file put the same rewrite rules that worked for me locally in that file and I am golden!  Not so fast  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/frownie.png" alt=":(" class="wp-smiley" style="height: 1em; max-height: 1em;" /> The rewrite rules I used locally were not working for me on BlueMix.

After trying various combinations of rewrite rules and not getting anywhere I decided I needed to figure out what the rewriten URLs actually looked like on the Apache server.  After Googling for a bit I figured out you could enable logging for the Apache rewrite module and it will tell you what it was doing.  Of course there were various ways of accomplishing this.  I decided to figure out what worked in my local dev environment first.  This took a while, but I eventually figured out that adding

<pre>&lt;IfModule rewrite_module>
  # Allow RewriteLog on localhost
  RewriteEngine On
  RewriteLog /Applications/MAMP/logs/rewrite.log
  RewriteLogLevel 5
&lt;/IfModule>
</pre>

to /Applications/MAMP/conf/apache/httpd.conf would enable the rewrite logging and print it to the logs/rewrite.log file.  Great, worked locally!  Now to the cloud and the buildpack, how do I do the same thing there?  After more trial and error there I figured out I could add

<pre>LogLevel alert rewrite:trace6</pre>

to my httpd-php.conf file and that would print out the rewrite logging to the sysout.log file on BlueMix.  I could finally see how Apache was rewriting my URLs!

After enabling that logging it took me a little while but I eventually came up with the right set of rewrite rules that were somewhat working.  I had the Apache server on BlueMix serving up my static HTML, JS, and CSS resources, but my Slim PHP app still was not working.  I kept getting 400s, and 404s all the time no matter what I did.  I eventually took a step back and went back and looked at the sample pointed to in the issue I mentioned above.  I realized that I did not have this line in my httpd-php.conf file.

<pre>ProxyPassMatch ^(.*)$ fcgi://#{PHP_FPM_LISTEN}${HOME}/htdocs/$1</pre>

This ended up being key.  I don&#8217;t completely understand everything about why I need this proxy rule, but the buildpack is using FastCGI and all the PHP code needs to be using that as well.  Basically this Apache proxy rule says to pass everything through FastCGI.  I did not want everything to go through FastCGI so I modified it slightly to only pass PHP files through.

<pre>ProxyPassMatch ^/?(.*php.*)$ fcgi://#{PHP_FPM_LISTEN}${HOME}/htdocs/$1</pre>

At this point I was finally able to get my Slim PHP app to work.  My REST endpoints were returning data!  It still bothers me that the rewrite rules that I have locally are different than the ones I need to use on BlueMix but at least it worked now.  (I still don&#8217;t understand why the local Apache rewrite rules don&#8217;t work with Apache on BlueMix, but that is another problem for another day.)

Next I wanted to be able to use Mongo DB from my PHP code.  I found this article about how to <a href="http://www.lukepeters.me/blog/post/setting-up-mongodb-with-php-and-mamp" target="_blank">install the Mongo PHP extension into MAMP</a>, which was really helpful.  This allowed me to write my code and test it locally.  After I had everything working locally I wanted to try it on BlueMix, but how do I get the Mongo extension installed in the buildpack on BlueMix.  I remembered reading in the documentation for the buildpack that it had support for Mongo, but how do I enable it?  After a little trial and error I eventually figured out that I needed to use the PHP_EXTENSIONS property in the <a href="https://github.com/dmikusa-pivotal/cf-php-build-pack/blob/master/docs/config.md#optionsjson" target="_blank">options.json file</a>.  I created my options.json file and added this to it

<pre>{
  "PHP_EXTENSIONS": ["bz2", "zlib", "curl", "mcrypt", "mongo"]
}</pre>

By default the buildpack include bz2, zlib, curl, and mcrypt, so I kept those there and then just added mongo.  When I pushed the app to BlueMix it now installed the Mongo DB PHP extension!

This all happened over the course of a work week.  It was frustrating at times and I am sure that part of it was because I am a novice at PHP, again this was my first time ever writing any PHP.  However most of my time was not spent trying to understand PHP, it was trying to figure out how to configure Apache.  This was quite frustrating to me because the whole point of using a PaaS like BlueMix was to not have to deal with all this configuration stuff, but I found myself spending 90% of my time figuring out how to configure things properly and only 10% coding.  IMO this isn&#8217;t so much a failure of the PaaS as it is the way the PHP runtime is tied to the container (web server).  So much of what I needed to do to get my app to work correctly was tied to making changes in the web server.  I have written the same app in Java, Node, Python, and Ruby, PHP was the only language where I had this experience.

Anyways if you are a PHP developer I hope the information above is useful and saves you time.  The code I wrote should be open sourced sometime in the beginning of April.