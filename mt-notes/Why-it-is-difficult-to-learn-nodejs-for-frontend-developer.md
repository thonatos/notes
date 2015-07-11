
为什么前端精通node.js的这么少？

那我们来梳理问题啊，前端通常做什么，后端要做什么？
不对不对，应该这么问：前端和后端分别要掌握哪些知识点以及分别要求有哪些认识？

#### 一个页面仔的日常

```
<!DOCTYPE html>
<html>
<head>
	<title>Example</title>
</head>
<body>

<table>
		<tbody>
		<tr class="caption">
			<td>name</td>
			<td>age</td>
			<td>sex</td>
		</tr>
		<tr class="item">
			<td>a</td>
			<td>1</td>
			<td>1</td>
		</tr>
		<tr class="item">
			<td>b</td>
			<td>2</td>
			<td>1</td>
		</tr>
				<tr class="item">
			<td>c</td>
			<td>3</td>
			<td>1</td>
		</tr>
	</tbody>
</table>

<div class="result">

</div>

<script type="text/javascript" src="http://cdn.bootcss.com/jquery/2.1.4/jquery.js"></script>
<script type="text/javascript">

	$(document).ready(function(){
		$('.item').click(function(e){

		var _items = $(this).children('td');
		var _id = _items[0].innerHTML;

		//alert(_items[0].innerHTML);
		getData(_id);

		});

		function getData(id){

			$.post("/data/post/",{id:id},function(response,status){

	            if(data && response.code === 200){

	                console.log(data);

	                $(".result").html(response.data);

	            }else{
	                console.log('err');
	            }
			});
		}
	})

</script>

</body>
</html>
```

上面这玩意的效果大概是当用户点击表格中的某一行的时候，从后台查询数据，然后添加到页面里。（随手写给后端的东西，请不要纠结细节~），

##### 基础知识

- html中table、tbody、tr、td使用，以及标签的class设置
- js中选择器的使用，jquery中ajax的post提交，标签内容的获取与修改

##### 错误处理

ajax提交数据后返回status，response，status通常是网络正常与否之类的东东，但是还有服务器返回数据的错误呢？
所以这边定义了一个response对象，结构大概是{code:200/500(int),data:{}}，然后对应了服务器内部查询的错误处理。

这些都比较简单，我这里只是举了一个特别简单的例子。
不过事实上呢，通常情况下，前端主要做的大概就是这个了~

（动画啊，特效啊，什么的，那也是！但是和nodejs这边关系不大，用expressjs加载几个页面什么の，乃不是精通！请不要跟我撕逼，谢谢。）

#### nodejs实例

前面是给后端哥哥写了一段便于他查询数据的简单代码，我们现在具体到实际开发喽，具体是一个“微信公众号“开发的小例子（只是粗略的写一写~）。

- 代理：nginx
- 语言：nodejs
- 框架：expressjs
- 存储：redis

微信公众号jssdk的接入，需要验证签名，获取token，接着拿到jsTicket,并最终生成签名。

##### 基础知识

###### Nginx

对于一个独立开发者，服务器的管理是必然的，况且，一个vps上面，你肯定要放很多蛋疼的小玩意，我们这边是nodejs来做基础环境，为了不影响其他服务的正常使用，最好是通过nginx前端代理，把请求转发到nodejs得web程序上面去，配置参考下面链接。（这里还有一堆进程守护什么的东西，随便听听就好了~）

https://www.thonatos.com/docs/backend-notes/Conf-App-Based-Express-With-Nginx-On-CentOs-7-x64.md

###### ExpressJs

不管你是用nodejs的http/https直接创建web程序，还是用expressjs框架，其实都是差不多的，对req,res的处理都一定要有所了解，有兴趣就去查官方api吧~

http://expressjs.com/4x/api.html

###### redis

为什么要这个呢？如果你看过微信的官方文档，应该知道签名相关的那些接口是有调用次数限制的，过期时间是7200s（2h），所以吧，肯定是要存在你服务器的，cookie、session之类的存储也不是不可以，我这里用redis来做缓存的。

##### 具体实现

暂定前面的那些你都搞定了或者你都用不到，我们直接进入实现的流程。

##### 关于结构、配置、部署等等

一个web服务器，路由肯定要有的，我这里是expressjs的，那么就是涉及到一个代码组织的问题，我一开始是使用expressjs生成的那种结构，大概是类似传统的mvc模式（不知道的自行搜索），但是问题来了，当你目录的层级到一定深度的时候，require()使用起来就蛋碎的一笔~

前一段时间爬了好久angular的坑（之所以说坑是因为1.3.x和2.x已经不是一个东西~），所以组织代码的方式变成了现在的这样样子，主要代码全部放在了app目录，资源文件放在public目录，conf那边就是调试和运行的一些配置文件了，app下面除了common那边以外是以功能来组织代码。

```
.
├── README.md
├── app
│   ├── api
│   ├── blog
│   ├── common
│   ├── doc
│   ├── fm
│   ├── index
│   ├── mood
│   ├── route.js
│   └── user
├── app.js
├── bin
│   ├── autoload.js
│   └── gulpfile.js
├── bower.json
├── conf
│   ├── api_develop.json
│   ├── api_product.json
│   ├── app_develop.json
│   └── pm2_develop.json
├── node_modules
├── package.json
├── public
│   ├── css
│   ├── css-gulp
│   ├── favicon.ico
│   ├── html
│   ├── images
│   ├── js-spm
│   └── vendors
└── static
```

也许有人觉得随便怎么放都行，是我事太多（好像我真的是个事b...），可是我发现啊，我改了好多次结构以后，现在用起来很方便，一目了然，给别人解释起来也比较方便（哼，咱目标可是架构，怎能实现功能就行？笑~），所以嘛，配置啊，模块啊，代码组织啊，运行啊，编译什么的，都要考虑嘛~，再说了，你要做nodejs啊，你是后端啊！你不考虑？不考虑部署？不考虑结构？不考虑配置？——行行行，你什么都不考虑，但是总得考虑这玩意公开以后别人能不能看得懂吧？

##### 其实写代码是最好做的那一部分~

先把所有请求全转到router那边来处理吧~

```
	# app.js
        // ------------- ROUTERS -------------
        require('./app/route')(app, passport);

	# router.js
        // Api
    var api   = require('./api/api');
    app.use('/api',api);

    # api.js
    // Router
    var apiApi = require('../api/apiModule').Api;
    router.route('/wechat/signature')
    	.get(apiApi.wechat().checkSignature);

	router.route('/wechat/signature/gen')
	    .get(apiApi.wechat().genSignature);

	router.route('/wechat/token/get')
	    .get(apiApi.wechat().getToken);
```


然后我们就来写代码吧~

```
	var obj = {};

	// 签名检查
    obj.checkSignature = function (req,res){

            var WECHAT_CONFIG = CONFIG_APP.weixin;
            var signature = req.query.signature;
            var timestamp = req.query.timestamp;
            var nonce = req.query.nonce;
            var echostr = req.query.echostr;

            var shasum = crypto.createHash('sha1');
            var arr = [WECHAT_CONFIG.token, timestamp, nonce].sort();
            shasum.update(arr.join(''));

            if(shasum.digest('hex') === signature){
                res.send(echostr);
            }else {
                res.send('err');
            }
    };

```


未完待续。

让我静静，后面的那些不忍心贴了，太长....