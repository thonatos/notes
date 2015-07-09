As we start this post, we may already know something about ng(AngularJs) like :

* ng-app
* ng-model
* ng-view
* ng-conroller
* how to add your res(css, js)
* how to add jquery, ng and other libs

So our html file may be :

```
	<!DOCTYPE html>
	<html xmlns="http://www.w3.org/1999/xhtml" ng-app="BIApp">
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
		<title>BigInt - Vzhibo.tv</title>
		<meta name="author" content="Vzhibo.tv" />
		<meta name="description" content="Statistics For BigInt - Vzhibo.tv" />
		<meta name="keywords"  content="Statistics,BigInt,Vzhibo.tv" />
		<meta name="Resource-type" content="Document" />
  		<link rel="stylesheet" type="text/css" href="public/css/vendors/bootstrap.min.css">
  		<link rel="stylesheet" type="text/css" href="public/css/vendors/bootflat.min.css">
  		<link rel="stylesheet" type="text/css" href="public/css/admin.css">
		<!--[if IE]>
		<script type="text/javascript">
			 var console = { log: function() {} };
		</script>
		<![endif]-->
	</head>
	<body ng-controller="generalController">
	<div class="header">
		<ul class="header-nav">
			<li><a href="#/" title="General">全站</a></li>
			<li><a href="#/event" title="Event">事件</a></li>
			<li><a href="#/post" title="Post">片段</a></li>
		</ul>
	</div>
	<div class="layout">
		<div ng-view></div>
	</div>
	<script type="text/javascript" src="public/vendors//jquery/1.9.1/jquery.min.js"></script>
	<script type="text/javascript" src="public/vendors/angularjs/1.2.25/angular.min.js"></script>
	<script type="text/javascript" src="public/vendors/angularjs/1.2.25/angular-route.min.js"></script>
	<script type="text/javascript" src="public/vendors/highcharts/highcharts.js"></script>
	<script type="text/javascript" src="public/vendors/bootstrap.min.js"></script>
	<script type="text/javascript" src="apps/admin/controller/app.js"></script>
	</body>
	</html>
```

### Deal with Route and MVC

_** MVC : Model and View, Controller **_

#### Define our app


```
var BIApp = angular.module('BIApp', ['ngRoute','BIConrollers']);
```

#### Define our route

```
BIApp.config(
	['$routeProvider',
	function($routeProvider) {
		$routeProvider.
		when('/', {
			templateUrl : __ROOT_VIEW__+'general.html',
			controller  : 'generalController'
		}).
		when('/event', {
			templateUrl : __ROOT_VIEW__+'event.html',
			controller  : 'eventController'
		}).
		when('/event/:view', {
			templateUrl: __ROOT_VIEW__+'event/event.html',
			controller: 'eventViewController'
		}).
		when('/post', {
			templateUrl : __ROOT_VIEW__+'post.html',
			controller  : 'postController'
		}).
		otherwise({
			templateUrl : __ROOT_VIEW__+'404.html'
		});
	}]
);

```

_** What we need to focus on is "/event/:view" **_

#### Define our Controller

```
var BIConrollers = angular.module('BIConrollers', []);
BIConrollers.controller('generalController', function($scope) {
	$scope.message = 'generalController';
});
BIConrollers.controller('eventController', function($scope) {
	$scope.message = 'eventController';
});
BIConrollers.controller('eventViewController', 
	['$scope', '$routeParams',
	function($scope, $routeParams) {
		$scope.eventView = $routeParams.view;
	}]
);
BIConrollers.controller('postController', function($scope) {
	$scope.message = 'postController';
});
```

_** eventViewConroller can deal with different requests. **_


### What are we doing ?

+ when html file has been loaded, 
+ **$routeProvider**    : get parms from **$routeParams**, 
+ **$routeProvider**    : load the **general.html**,turn to **generalController**
+ **generalController** : put data 
+ **general.html**      : get data and render html

this is traditional way, what is special? click the nav menu, it will do like about, but html file will not be load again! Just parts of DOM will be changed, that's what we have done! 

I do believe you can do it by using kinds of methods with JavaScript, But, what we do just now is so easy, we do not need to "find","replace" or some new methods, just define var in html files and put them somewhere, everything will be loaded automatic, Besides, you may not pay attention to **Browser Address Bar**, it has changed without page loaded again!

we will find more later.

贴代码这种很简单暴力的方式虽然很没有节操，但说真的，很实在很有用，至少能让人一眼就明白怎么用不是？