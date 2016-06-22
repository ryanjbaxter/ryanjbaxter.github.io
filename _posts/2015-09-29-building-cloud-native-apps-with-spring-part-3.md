---
id: 1115
title: Building Cloud Native Apps With Spring – Part 3
date: 2015-09-29T12:41:56+00:00
author: ryanjbaxter
layout: post
guid: http://ryanjbaxter.com/?p=1115
permalink: /2015/09/29/building-cloud-native-apps-with-spring-part-3/
categories:
  - Cloud
  - Cloud Foundry
  - PaaS
---
In Parts <a href="http://ryanjbaxter.com/2015/09/14/building-cloud-native-apps-with-spring-part-1/" target="_blank">1</a> and <a href="http://ryanjbaxter.com/2015/09/21/building-cloud-native-apps-with-spring-part-2/" target="_blank">2</a> of this tutorial we have built an application which displays upcoming obstacle course races and the participants that will be running them.  We have addressed some common cloud native app challenges list cross domain requests and service discovery by using projects from Spring Cloud and Netflix, namely Eureka and Zuul.  In part 3 we will finish implementing the web UI of our application.

First lets make sure that we can fix the cross domain issue we were having when we started.

## Implementing The Races View

Open _main.js_ in the _src/main/resources/static/controllers_ folder of your web service.  Change the _url_ property of the _$http_ call to _/races_.

~~~JavaScript
angular.module('ocrApp')
  .controller('MainCtrl', function ($scope, $http) {
	  $http({
          method: 'GET',
          url: '/races'
      }).then(function(response) {
    	  $scope.races = response.data;
      }, function(response) {
    	  console.error('Error requesting races');
      });
  });
~~~

Make sure all the services are running and registered with Eureka and go to <a href="http://localhost:8080" target="_blank">http://localhost:8080</a>.  If you open the console of your browser you should no longer see any cross domain errors and your request to <a href="http://localhost:8080/races" target="_blank">http://localhost:8080/races</a> should complete successfully.

Now lets leverage the data we are getting back from the request in the view for the main controller.  Open _main.html_ in the _src/main/resouces/static/views_ folder of the web service and change to code to be

~~~html
<h3>Races</h3>
<ul>
	<li ng-repeat="race in races"><a ng-href="/#/participants/{{"{{race.id"}}}}">{{"{{race.name"}}}}</a></li>
</ul>
~~~

After saving this file, go to <a href="http://localhost:8080" target="_blank">http://localhost:8080</a> and you should see a list of races

[<img class="alignnone size-full wp-image-1116" src="http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-10-at-3.55.59-PM.png" alt="Screen Shot 2015-09-10 at 3.55.59 PM" width="774" height="274" srcset="http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-10-at-3.55.59-PM-300x106.png 300w, http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-10-at-3.55.59-PM.png 774w" sizes="(max-width: 774px) 100vw, 774px" />](http://ryanjbaxter.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-10-at-3.55.59-PM.png)

Clicking the links won&#8217;t work yet because we have not added the views or controllers to render the page, lets do that now.

## Adding A Participants View

Create a new file called _participants.js_ in _src/main/resources/static/scripts/controllers_ and add the following code

<pre class="font-size:15 lang:default decode:true">angular.module('ocrApp')
  .controller('ParticipantsCtrl', function ($scope, $http, $routeParams) {
	  $http({
          method: 'GET',
          url: '/participants/races/' + $routeParams.raceId
      }).then(function (response) {
    	  $scope.participants = response.data;
      }, function(response) {
    	  console.error('Error requesting participants.')
      });
  });</pre>

This code just makes a request to our participants service (through Zuul) to get the list of participants for the race we clicked on.  Now we need a view to render the participants names.  Create a file called _participants.html_ in _src/main/resources/static/views_ and add the following code

<pre class="font-size:15 lang:default decode:true ">&lt;h3&gt;Participants&lt;/h3&gt;
&lt;ul&gt;
	&lt;li ng-repeat="participant in participants"&gt;{{"{{participant.firstName"}}}} {{"{{participant.lastName"}}}}&lt;/li&gt;
&lt;/ul&gt;</pre>

Finally we need to tell Angular about the new participants view and controller.  Open _app.js_ in _src/main/resources/static/scripts_ and modify it so it looks like the following code

<pre class="font-size:15 lang:default decode:true ">angular
  .module('ocrApp', [
    'ngAnimate',
    'ngCookies',
    'ngResource',
    'ngRoute',
    'ngSanitize',
    'ngTouch'
  ])
  .config(function ($routeProvider) {
    $routeProvider
      .when('/', {
        templateUrl: 'views/main.html',
        controller: 'MainCtrl'
      })
      .when('/participants/:raceId', {
    	  templateUrl: 'views/participants.html',
    	  controller: 'ParticipantsCtrl'
      })
      .otherwise({
        redirectTo: '/'
      });
  });</pre>

Last but not least we need to include _participants.js_ in our _index.html_ page so the new JavaScript gets loaded.  Open i_ndex.html_ in _src/main/resources/static_ and at the bottom, right after all the other script tags, add the following script tag to the file

<pre class="font-size:15 lang:default decode:true ">&lt;script src="scripts/controllers/participants.js"&gt;&lt;/script&gt;</pre>

Thats it!  Refresh the page and try clicking on the links, they should all work now and you should be able to see every participant in each race.

Congratulations you have built your first microservice app with Spring Boot and Spring Cloud!  Granted it is very simple and only works on your local machine, but it didn&#8217;t take much effort to do because Spring Boot and Spring Cloud make it easy to do what otherwise would be very complex tasks.
