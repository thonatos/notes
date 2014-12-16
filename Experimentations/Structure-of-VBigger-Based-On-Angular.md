VBigger前端基于AngularJS（以下简称ng）框架构建，后端通过Api为前端提供所需数据，实现了前后端分离，特点如下：

* 前端MVC（基于ng的MVVM/MVC的编程模式）
* 自动构建（基于gulp.js的less/sass自动构建方案,此处可以也可以实现css,js,图像等资源的合并压缩）
* 模块化开发（基于sea.js的模块化构建方案）
* 模块化管理（业务模块通过spm.js为sea.js提供符合CMD规范的组件，bower.js管理常用插件和模块的管理）

### 项目架构

前端架构设计总体上依赖系统架构进行设计。

### 项目结构

```
.
├── Application (Back-End）
├── apps
│   ├── dist
│   │   └── bigint
│   ├── spm_modules
│   │   ├── expect.js
│   │   ├── jquery
│   │   ├── seajs
│   │   └── seajs-wrap
│   └── src
│       ├── components
│       ├── event
│       ├── general
│       └── post
├── bower.json
├── index.html
├── gulpfile.js
├── node_modules
├── package.json
├── public
│   ├── css
│   │   ├── dev
│   │   ├── fonts
│   │   ├── less
│   │   └── vendors
│   ├── images
│   └── vendors
│       ├── bower_components
│       └── custom_components
└── ThinkPHP3.2.2 (Back-End）

## 未说明的部分皆属于前端。

```

### 前端结构

前端分为apps和public两部分，apps为前端系统目录，public为资源文件目录，其余为构建与管理的相关配置文件/脚本。

依照项目系统架构将前端系统分为以下模块：

* general（全站）
* event（事件）
* post（片段）
* components(共有组件)

三个层级进行模块设计和开发，目录结构如下：

```
.
├── app.js
├── components
│   ├── conf
│   │   └── conf.js
│   ├── directive
│   │   ├── chartDirective.html
│   │   ├── devDirective.html
│   │   ├── errorDirective.html
│   │   ├── pagerDirective.html
│   │   ├── searchDirective.html
│   │   └── sidebarDirective.html
│   ├── example
│   │   ├── component.js
│   │   ├── main.js
│   │   └── model.js
│   ├── factory
│   │   └── directive.js
│   ├── service
│   │   ├── ajaxService.js
│   │   ├── service.js
│   │   └── shareService.js
│   ├── util
│   │   ├── fmtChart.js
│   │   ├── fmtTool.js
│   │   ├── logger.js
│   │   └── WordCloud.js
├── event
│   ├── event.js
│   ├── layout.html
│   ├── main.eventSlotViewCount.html
│   ├── main.html
│   ├── main.keywordInfo.html
│   ├── main.onlineTotal.html
│   ├── main.postInfo.html
│   └── main.userAccessMethod.html
├── general
│   ├── general.js
│   ├── layout.html
│   ├── main.eventsList.html
│   ├── main.eventViewCount.html
│   └── main.html
└── post
    ├── layout.html
    ├── main.breakSource.html
    ├── main.html
    └── post.js

```

### 设计思想

为实现更灵活自主的配置，依照模块化开发的思想，采用了**模块内部定义配置**方式：   
自主定义路由和视图，**分离通用模块**统一组织在components下进行管理，**app.js**作为程序入口，仅引入并初始化相关模块，配置皆在模块内部配置。

### 设计实现

#### 模块

以general为例，通过app.js（入口）引入general，并手动启动（bootstrap，这里主要是区别于在HTML中加入ng-app="ASS"这样的自动引导方式），并在general内部进行配置general.config()，代码上的体现为：

```
general.config(['$stateProvider', '$urlRouterProvider', function ($stateProvider, $urlRouterProvider) {
    $stateProvider
        .state('general', {
            url: '/general',
            views: {
                '': {
                    templateUrl: 'apps/src/general/layout.html',
                    controller: 'generalCtrl'
                },
                'main@general': {
                    templateUrl: 'apps/src/general/main.html',
                    controller: 'generalMainCtrl'
                }
            }
        })
        .state('general.eventViewCount', {
            url: '/eventViewCount',
            views: {
                'main@general': {
                    templateUrl: 'apps/src/general/main.eventViewCount.html',
                    controller: 'generalViewEventViewCountCtrl'
                }
            }
        });
}]);
```

* 路由  --> 采用angular-ui-router第三方ng插件实现更灵活的路由配置
* 视图  --> 将视图分解为单独的模块，ui-view="main"为模块下变化的部分（命名为main.xxx.html），不变内容定义在layout.html中

* 控制器 --> 
    * 引入了layout.html做模块下的通用配置，故而使用了一个“根”级别的controller进行通用配置。
    * layout.html中ui-view=“main”定义基于main@general的”次“级别controller进行非通用的方法或功能。
 
#### 指令

ng指令，在具体的功能中体现为一个类div的标签，在系统中将一些通用的功能封装成指令，在系统内部皆可调用，例如sidebar-Directive。

* components/directive/

#### 服务

将前端App中的一些通用或者全局的内容作为服务，例如ajax，提供setDataJson()或者getDataJson()给外部调用，以此异步获取数据。

**# 这里使用异步请求获取数据，数据获取完成后使用$broadcast进行广播，在当前ctrl中监听广播，收到广播后进行绘制。**

**# share提供了项目公用的eid和pid的get/set方法（这里会修改，更多全局的内容会放进这里）**

* /components/service/

#### 工具

项目目前比较简单，并未包含很多内容，工具主要是提高对数据二次处理的一些方法。