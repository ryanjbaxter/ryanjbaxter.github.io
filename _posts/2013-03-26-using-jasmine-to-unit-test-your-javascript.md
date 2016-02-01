---
id: 401
title: Using Jasmine To Unit Test Your JavaScript
date: 2013-03-26T21:39:54+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=401
permalink: /2013/03/26/using-jasmine-to-unit-test-your-javascript/
categories:
  - Web Development
---
Lately I have been writing quite a few Dojo AMD modules for a project I am working on.  These modules include Dojo widgets and API level code.  Being a good developer  <img src="http://ryanjbaxter.com/wp-includes/images/smilies/simple-smile.png" alt=":)" class="wp-smiley" style="height: 1em; max-height: 1em;" />I wanted to make sure I could test the code I was writing.  I had worked with other JavaScrupt unit testing frameworks in the past and had been frustrated by them because there was no real good &#8220;framework&#8221; built around them.  You could do the basics that you would expect, like running some setup and cleanup code but there was no framework for mocking up objects or inspecting other aspects of the code you were testing.  I had heard good things about a framework called <a href="http://pivotal.github.com/jasmine/" target="_blank">Jasmine</a> recently and decided I would check it out.

Getting started was pretty easy.  Your unit tests are split up into different suites.  Each suite describes all the functionality a specific feature is meant to provide.  So say I have the ability to Tweet from my app and I want to test it.  Here is what my Jasmine test would look like

<pre>describe('My Twitter feature', function() { 
  it('can post a tweet', function() { 
    //Add your code to test the tweet functionality 
  }); 
}); 
</pre>

Notice how the suite describes the functionality of the feature.  When Jasmine runs the suite it will print something like &#8220;My Twitter feature can post a tweet.&#8221;  So by reading the test results you can easily tell what functionality each feature has.

The mocking capabilities of Jasmine are excellent!  Jasmine not only lets you mock methods/functions of existing code but if you need to mock an entire object you can do that as well.  In addition to mocking anything you want/need you can &#8220;spy&#8221; on them as well.  By spying on a method/function you can tell which parameters were passed, how many times the method/function was called, and even the types of the parameters passed.

Running the tests are pretty easy as well, you just start up your browser and load a page containing the Jasmine framework and your tests and Jasmine will run them.  Obviously you want a more automated solution for doing this and there are various ways of doing that as well.  I have been using Maven in the project I am working on and there is a <a href="http://searls.github.com/jasmine-maven-plugin/index.html" target="_blank">Jasmine Maven plugin</a> you can include in your project to run your Jasmine tests using Maven.  The great thing about the Maven plugin is that while you are developing your tests there is a Maven goal you can run that starts up a local server hosting a page that will run your tests. As you write your tests all you have to do is refresh the page and your new tests are run. This makes it very easy develop your tests.  The Maven plugin will also run these tests as part of your Maven build but in this case they run in a &#8220;headless&#8221; mode where a physical browser is not needed.

If you are writing JavaScript in your project I encourage you to check out Jasmine for unit testing your code.  I have been more than satisfied with it.