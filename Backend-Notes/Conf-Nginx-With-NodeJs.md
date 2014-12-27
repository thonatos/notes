> * title: Nginx+Node.js环境配置
> * date: 2013-01-30 05:41:19


最近要给团队搭建一个网站，鉴于近期学习Js比较多，因而考虑用Node.js做开发，

与此同时，顺带着安装了Nginx，算是搭建一个Nginx+Node.js环境吧~


#### 资源文件

* Node.js		http://nodejs.org/
* Nginx		http://nginx.org/

#### 环境配置

##### Nginx配置

其实Nginx的配置没什么好说的，我下载的是Stable version，也就是1.6，zip的解压并更名为nginx即可，之前下载过一个小工具：

RunHiddenConsole.exe，配合着写一段批处理：
	
	@echo off
	echo 正在启动Nginx进程......
	%~dp0RunHiddenConsole.exe E:My-ToolsNginxnginx.exe -c E:My-ToolsNginxconfnginx.conf
	echo Nginx已启动。
	pause


关于conf的配置根据个人需要改文件conf/nginx.conf:

	server {
		listen       80;
		server_name  localhost;

		#charset koi8-r;

		#access_log  logs/host.access.log  main;

		location / {
			root   html;
			proxy_pass http://127.0.0.1:8080;
		}

		error_page  404              /50x.html;
	}


运行时候点击上面的写的批处理即可。

##### Node.js配置

将下载好的node.js解压并设置全局变量（这个，个人百度吧....），我配置的目录就是那个html了。


#### 最后测试

依然是写一个juy.js文件吧，内容如下：

	var http = require('http');
		http.createServer(
			function (req, res) {
				res.writeHead(200, {'Content-Type': 'text/plain'});
				res.end('Hello Juy n');
			}
		).listen(8080, "127.0.0.1");
	console.log('Server running at http://127.0.0.1:8080/');

接着命令行运行:

	node juy.js
	
打开浏览器http://localhost，熟悉的hello Juy就出现了，大功告成。