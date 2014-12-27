> * title: Nginx+PHP+Mysql环境配置
> * date: 2014-05-18 14:53:26


升级win8后感觉iis的使用还是比较方便的，偷懒什么的就一直在用iis，但是最近感觉内存吃紧，再加之笔者对电脑有点小洁癖，所以花费几分钟配置了一下Nginx+PHP+Mysql的运行环境，便于笔者进行php程序的测试及调试。

#### 所需文件

*   nginx-1.6.0		[http://nginx.org/download/nginx-1.6.0.zip](http://nginx.org/download/nginx-1.6.0.zip)
*   php-5.4.28		[http://windows.php.net/downloads/releases/php-5.4.28-Win32-VC9-x86.zip](http://windows.php.net/downloads/releases/php-5.4.28-Win32-VC9-x86.zip)
*   mysql-5.6.17	[http://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.17-win32.zip](http://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.17-win32.zip)
*   RunHiddenConsole	[http://redmine.lighttpd.net/attachments/660/RunHiddenConsole.zip](http://redmine.lighttpd.net/attachments/660/RunHiddenConsole.zip)


#### 环境配置

解压对应的压缩包并得到相应的文件，php的相关配置根据自己的需要来写，mysql的配置麽，也就不多说了，这些都是基本的，其他地方基本都写了，直接跳过上述步骤来说具体的配置。

在nginx的conf目录下新建php.conf，然后修改nginx.conf为如下内容：

	#user  nobody;
	worker_processes  1;
	#error_log  logs/error.log;
	#error_log  logs/error.log  notice;
	#error_log  logs/error.log  info;
	#pid        logs/nginx.pid;
	
	events {
    	worker_connections  1024;
	}

	http {
    	include       mime.types;
    	default_type  application/octet-stream;
		#header-charset
		charset utf-8;

    	#log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    	#                  '$status $body_bytes_sent "$http_referer" '
    	#                  '"$http_user_agent" "$http_x_forwarded_for"';

    	#access_log  logs/access.log  main;

    	sendfile        on;
    	#tcp_nopush     on;

    	#keepalive_timeout  0;
    	keepalive_timeout  65;

    	#gzip  on;

		gzip  on;

		#autoindex
		autoindex on;
		autoindex_exact_size on;
		autoindex_localtime on;

		server {
			listen       80;
			server_name  localhost;

			#charset koi8-r;

			#access_log  logs/host.access.log  main;

			location / {
            	root   F:/My_Workspace/Web_Root;
            	index  index.php index.html index.htm;
            	#添加index.php的默认页
        	}

        	#error_page  404              /404.html;

			error_page  404              /404.html;

			# redirect server error pages to the static page /50x.html
        	#
        	error_page   500 502 503 504  /50x.html;
        	location = /50x.html {
            	root   html;
        	}

        	#此处进行php的配置，兵引入之前建立的php.conf文件
        	location ~ \.php$ {
            	root           F:/My_Workspace/Web_Root;
            	include        php.conf;
        	}

        	# deny access to .htaccess files, if Apache's document root
        	# concurs with nginx's one
        	#
        	#location ~ /\.ht {
        	#    deny  all;
        	#}
    		}
		}


然后对php.conf进行修改：

	#此处设置php的fastcgi路径
    fastcgi_pass   127.0.0.1:9000;
	#此处设置默认页面
	fastcgi_index  index.php;
	#此处设置运行脚本
	fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
	include        fastcgi_params;
	
上述基本已经配置好了php的运行环境的一些设置，然后新建start_nginx.bat与stop_nginx.bat的批处理文件并将RunHiddenConsole放在nginx的根目录下，相关的内容如下：

	#start_nginx.bat

	@echo off
	echo Starting PHP FastCGI...
	RunHiddenConsole.exe D:/My-Tools/php-5.4.27/php-cgi.exe -b 127.0.0.1:9000 -c 	D:/My-Tools/php-5.4.27/php.ini
	echo Starting nginx...
	RunHiddenConsole.exe D:/My-Tools/nginx-1.6.0/nginx.exe
	exit

	#stop_nginx.bat
	@echo off
	echo Stopping nginx...
	taskkill /F /IM nginx.exe &gt; nul
	echo Stopping PHP FastCGI...
	taskkill /F /IM php-cgi.exe &gt; nul
	exit
	
至此运行环境基本完成，访问localhost就可以访问了，其他内容就不再多说了，核心的东西就这么多了。