
### Basic Knowledge

As we start this post, we may still heve to know something about ng(AngularJs) and seajs, SeaJs - A Module Loader for the Web, ng(AngularJs) is a MVVM(Model View ViewModel) framework:


* how to add your res(css, js)
* how to add jquery, ng and other libs
* how to configure SeaJs and ng


* Official SeaJs: [http://seajs.org/](http://seajs.org/)
* Official ng Document: [https://angularjs.org/](https://angularjs.org/) 


_** [angularjs-route-template](http://tech.thonatos.com/angularjs/angularjs-route-template/) **_ 


### Basic Html

if you know how to include libs and add basic conf, your html file will be:

```
	<!DOCTYPE html>
	<html xmlns="http://www.w3.org/1999/xhtml">
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
	<script type="text/javascript" src="apps/sea-modules/seajs/2.3.0/dist/sea.js"></script>
	<script type="text/javascript" src="apps/sea-modules/jquery/2.1.1/jquery.min.js"></script>
	<script type="text/javascript" src="public/vendors/angularjs/1.2.25/angular.min.js"></script>
	<script type="text/javascript" src="public/vendors/angularjs/1.2.25/angular-route.min.js"></script>
	<script type="text/javascript" src="public/vendors/highcharts/highcharts.js"></script>
	<script type="text/javascript" src="public/vendors/bootstrap.min.js"></script>
	<script>
		seajs.config({
			base: "./apps/sea-modules/"
		})
    	seajs.use('./apps/src/admin/app')
	</script>
	</body>
	</html>
```

if you compare this with last post i wrote, you may know we remove tags -- ** ng-app="BIApp" **,
because we combine SeaJs and ng, so ng should not be loaded automaticly, everything is controlled by modules of SeaJs,
here, it's the file -- ** ./app/src/admin/app.js **, then we will say how it works.


### JavaScript(Main App)

as you known, we removed ** ng-app="BIApp" ** and bootstrap ng by code in ** ./app/src/admin/app.js **, so, how to ?


_** Before we start this section, we need know basic format for CMD used by SeaJs **_


```
// All modules should be defined by "define"
define(function(require, exports, module) {
  // use "require" to include dependencies and modules
  var $ = require('jquery');
  var Spinning = require('./spinning');
  // use "exports" to share interface
  exports.doSomething = ...
  // use "module.exports" share whole interface
  module.exports = ...
});
```

then our main app:

```
define(function (require, exports, module) {
	/*  *********************************
	 	*	GLOBAL STATIC VARS
	 	********************************  */		
	var __ROOT__ = 'apps/src/admin/';
	var __ROOT_VIEW__ = __ROOT__+'view/';
	/*  *********************************
	 	*	APP 	Main
	 	********************************  */
	var BIApp = angular.module('BIApp', ['ngRoute','BIConrollers']); 
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
	var BIConrollers = angular.module(require("./controller/app"), []);
	angular.bootstrap(document, ['BIApp']);
});
```

* Define some static vars
* Instantiate our ng app
* Configure ng ** $routeProvider **
* Define ** controller **
* Run our app -- ** ng app** from _** document **_

our controllers:

```
define(function(require, exports, module) {
	/*  *********************************
	 	*	APP CONTORLLER
	 	********************************  */	
	var BIConrollers = angular.module('BIConrollers', []);
	BIConrollers.controller('generalController', function($scope) {
		$scope.message = 'generalController';
	});
	BIConrollers.controller('eventController', function($scope) {
		$scope.message = 'eventController';
	});
	module.exports = BIConrollers;
});
```

the most important place is: _** module.exports = BIConrollers; **_, with it, we can _** require **_ controllers here.

we will find more later.
