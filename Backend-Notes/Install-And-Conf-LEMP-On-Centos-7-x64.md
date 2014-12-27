> * title: CentOS 7.0 x64下LEMP环境配置
> * date: 2014-08-19 04:56:54

团队搭建网站，故而换了Vps作为服务器使用，显而易见，环境配置就是第一步了，综合考虑之后选择了CentOs 7.0 x64做系统，团队网站的话是使用LEMP（Linux+Nginx+Mysql+Php），老高那边也弄了一台，但他没怎么接触过linux系统，当然也就不要说环境配置了，所以整理下内容，方便同样对linux不太熟悉的朋友们。

### 服务器初始化

首先讲一下为什么要初始化配置，很多新人（我也是其中之一）都习惯性的在root权限下操作服务器，这个习惯确实是不太好，虽然说是很方便，但是吧，出了问题往往根本不知道出在哪里，所以还是建议按照我的推荐内容，对服务器做下初始化配置，大体就是配置一个普通帐号来使用，只有必要的时候才使用root帐号。

#### 登录服务器：

    ssh root@domain.name

    ## 更改密码：
    passwd
    
    ## 添加用户：
    adduser thonatos

    ## 设置密码：
    passwd password

    ## 添加权限：
    visudo
    
    ## 然后找到下面的内容，并新添加一行:
    # User privilege specification
     root    ALL=(ALL)       ALL
     thonatos    ALL=(ALL)       ALL
     
    ##（这里说一下vi吧，其实我也不太会，但是勉强还能用一点，进入以后，键盘i进入插入模式，
    ## 不进行输入的时候就esc，然后切换到你要输入的位置，保存退出的话是键盘输入:wq并且回车键退出）

    ## 修改ssh端口是必要的，最起码默认的端口，还是尽量改了吧
    sudo nano /etc/ssh/sshd_config`</pre>
    Port 25000
    PermitRootLogin no
    AllowUsers thonatos
    
    ## 接着重启ssh服务，
    sudo systemctl reload sshd.service
    
    ## 测试的话用下面这句，如果没问题，可以登录，那就可以退出了~
    ssh -p 25000 thonatos@domain.name



### 运行环境配置

上面说完了初始化，现在应该要搭建LEMP环境了，由于默认的yum源中nginx比较老，想要使用新的版本需要添加源，（注意我们现在是使用刚才配置的新帐号登录的，换言之，不是root帐号，所以需要输入sudo）

#### 配置CentOs7的yum源

	sudo rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm

#### 安装nginx

	sudo yum install nginx

等待完成以后，启动服务看看：

	sudo systemctl start nginx.service
	sudo systemctl enable nginx.service (开机启动)
	
接着就可以浏览器打开看看了，如果看到欢迎信息，就可以继续下一步了。

#### 安装Mysql

	sudo yum install mariadb-server mariadb

同样的，等待完成后启动mysql数据库：

	sudo systemctl start mariadb
	
接着可以进行mysql的初始化配置，设置密码帐号，删除测试表（这里基本是都输入y）：

	sudo mysql_secure_installation
	
按照提示设置数据库的密码即可，同理配置开机启动：

	sudo systemctl enable mariadb.service
	
#### 安装php-fpm

	sudo yum install php php-mysql php-fpm
	
同样的基本都是等待完成即可，然后继续配置php：

	sudo nano /etc/php.ini

然后修改按照自己的需要去掉注释“;”(快速定位内容请使用ctrl+w)：

	cgi.fix_pathinfo=0
	
接着ctrl+o保存，ctrl+x退出编辑，并继续编辑php-fpm如下：

	sudo nano /etc/php-fpm.d/www.conf
	
	## 找到listen并改为如下内容：
	listen=/var/run/php-fpm/php-fpm.sock

（之前忘了说了，感谢老高测试，哈哈哈）这里补充一句,用户组是要改了的，不然运行起来是有问题的：

	user = nginx 
	group = nginx 

接着启动服务，没有问题的情况下，开启开机启动：

	sudo systemctl start php-fpm
	sudo systemctl enable php-fpm.service


#### 配置nginx+php-fpm

这里抽时间单独拿出来讲配置文件的写法，现在只是为了测试环境是否搭建成功，所以采用简略的写法：

	sudo nano /etc/nginx/conf.d/default.conf
	
去除注释以后，大概是这个样子的：

	server {
    	listen       80;
    	server_name  localhost;

    	location / {
        	root   /usr/share/nginx/html;
        	index  index.html index.htm;
    	}
    	error_page   500 502 503 504  /50x.html;
    	location = /50x.html {
        	root   /usr/share/nginx/html;
    	}
	}

然后我们修改成下面的样子：
	
	server {

    	# 监听端口，默认就是80
    	listen       80;

    	# 网站域名
    	server_name  server_domain_name_or_IP;

    	# 网站根目录(我是改成了/home/wwwroot)
    	root   /usr/share/nginx/html;
    	index index.php index.html index.htm;

    	location / {
        	try_files $uri $uri/ =404;
    	}

    	# 错误页配置
    	error_page 404 /404.html;
    	error_page 500 502 503 504 /50x.html;
    	location = /50x.html {
        	root /usr/share/nginx/html;
    	}

    	# php-fpm配置，即对于php后缀的，都是用php-fpm处理
    	location ~ \.php$ {
        	try_files $uri =404;
        	fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        	fastcgi_index index.php;
        	fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        	include fastcgi_params;
    	}
	}

这里都改完以后就可以保存了，然后在你的根目录（上面设置的根目录，比如我的/home/wwwroot）下面新建一个index.php文件并写入：

	<?php phpinfo();?> ##（斜杠全部去掉。）
	
这里要说一下啊，我们的现在使用的帐号和用yum安装nginx的用户是不一样的，换言之，就是直接写入的文件会读取不到，所以再改权限：

	sudo chmod -R 755 /home/wwwroot/*
	sudo chown -R nginx.nginx /home/wwwroot/*

或者这样输入：

	cd /home/wwwroot
	sudo chmod -R 755 ./
	sudo chown -R nginx.nginx ./


#### 环境测试

之前修改了nginx的配置，所以重启下nginx：

	sudo systemctl restart nginx.service

接着打开你的网站看看效果吧，不出意外的话，应该可以看到服务器信息了。