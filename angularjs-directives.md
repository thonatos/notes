### categories : angularjs

AngularJS(ng) lets you extend HTML with new attributes called Directives. --W3Schools.

ng用来扩展HTML的属性即指令，通过ng的指令，我们能够对HTML进行扩展，ng自带了一些指令，例如：

* ng-app
* ng-init
* ng-click
* ng-model
* ...

通过ng自带的指令，我们可以实现很多非HTML原生的功能，与此同时，我们也可以通过ng自定义指令，来实现我们所需要的功能。

### ng官方指令

* ng-app指令用来初始化ng程序
* ng-init指令用来初始化程序数据
* ng-model指令用来绑定HTML的元素到ng程序数据


例子：

```
	<div ng-app="ng-directives" ng-init="name='tt'">
    	<p>{{ name }}<p>
    	<p>Name: <input type="text" ng-model="name"></p>
	</div>
```	

这个简单的程序默认的显示tt，当我们改变输入框的内容，显示的内容也随之改变（例如我们输入name，则显示的内容也变为name）。

### 自定义指令

ng自带的指令很多，也很实用，但是不一定能够适合我们在开发环境中的使用，例如我们需要在一个div中动态的改变内容，使用ng自带的指令，显然需要写很多才能够实现，
我们需要一个更具有扩展性的方式来实现，ng为我们提供了这样的方法或者说是这样的能力，开篇就讲到了，ng可以让我们通过新的属性对HTML进行扩展。

注册一个一个新的指令方法比较多，但是实现是基本类似的，不过是包裹的方式不一样，但功能是一样的：


#### basis directive


js代码：

```
var ass = angular.module('ASS',[]);
ass.directive('newDirective',function(){
    return {
        restrict: 'E',
        template: '<div>ng-directive</div>',
        replace: true
    };
});
```

html代码：


```
<html ng-app="ASS">
    <body>
         <new-directive></new-irective>
    </body>
</html>
```
这样html中的<new-directive></new-directive>就会现实为ng-directive。


#### directive - templateUrl


当然，这里是最简单的实现，上面的js还可以换成一个html页面。例如：
js代码：

```
var ass = angular.module('ASS',[]);
ass.directive('newDirective',function(){
    return {
        restrict: 'E',
        templateUrl: 'newDirective.html',
        replace: true
    };
});
```

newDirective.html可以写成这样：

	<div>ng-directive</div>

然后输出的结果是相同的，不过只是这样还远远不够，仅仅替换内容，我们自己用js实现也很容易，何必用这么复杂的代码？


#### directive - transclude


我们来进一步增加他的功能，还是贴代码，简单暴力。
js代码：

```
var ass = angular.module('ASS',[]);
ass.directive('newDirective',function(){
    return {
        restrict: 'E',
        templateUrl: 'newDirective.html',
        transclude: true
    };
});
```
newDirective.html可以写成这样：


```
	<div>ng-directive : <p ng-transclude></p> </div>
```


```
<html ng-app="ASS">
    <body>
         <new-directive><p>message</p></new-irective>
    </body>
</html>
```


这样就会将HTML中自定义的指令内部的内容引入了。


#### directive - controller

这样还不够，他只是实现了html标签的功能，肯定要做点不一样的事情么，不然要你何用？

js代码：


```
directiveFactory.directive('searchDirective', function () {
    return {
        restrict: 'E',
        templateUrl: __ROOT__ + 'components/directive/searchDirective.html',
        controller: function ($scope, shareService) {
            //事件处理
            $scope.setEvent = function (eid, pid) {
                if (angular.isNumber(eid)) {
                    shareService.setEid(eid);
                }
                if (angular.isNumber(pid)) {
                    shareService.setPid(pid);
                }
                var _eid = shareService.getEid();
                var _pid = shareService.getPid();
                if (angular.isNumber(_eid) || angular.isNumber(_pid)) {
                    console.log("SET GLOBAL EID : " + _eid + " PID : " + _pid + " AND REDIRECT TO EVENT(URL)");
                    location.href = '#/event/';
                }
                $('#searchModal').modal('hide');
            };
        }
    };
});
```

这里我贴一个写过的directive，他能做什么呢？   
首先他内部定义了一个controller，这个controller包含一个$scope，这个$scope可以用来想我们的tmpl中绑定和传递参数，同时也能够接收tmpl中的event，例如这里我定义了一个setEvent方法，他可以用来传递用户选择eid和pid，  其次我向controller中注入了一个shareService，这里就能够将eid和pid传递给
我们定义的公共服务，让其他的controller获取eid/pid了。

那么我们总结一下，directive可以实现的功能包含了：

* 替换自定义的HTML标签
* 附加标签内容到新的标签
* 事件绑定
* 参数绑定
* 注入调用（这里可以是服务，当然也可以是其他的一些内容，比如factory，又或者ng自带的一些功能，如$http,$route等等）

### More

暂时就写这么多了，有补充的再添加，凌晨两点半的南京，静悄悄，该睡觉了哦~