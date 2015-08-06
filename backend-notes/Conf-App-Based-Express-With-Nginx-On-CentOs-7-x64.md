> 配置基于Express,Nignx的Web App的进程守护及开机重启


文档站基于ExpressJs跑在VPS，使用了Nginx做前端代理，转发请求到内部网络的NodeJs服务器上。

为了能让开机启动，并在文件变化（这个情况比较少吧）或者进程意外结束后重启，参考了诸多的解决方案：

* 基于Supervisord的自启动方法
* 基于Forever与Crontab的方式
* 基于Systemd的自定义服务启动
* 基于PM2的进程守护，宕机重启
 
试用了上述的几种方式，supervisor适用于开发环境；forever据说是内部包含了一个supervisor的模块，具体的没怎么看，但是它的日志，是比较烦人的，不方便查看；pm2刚刚安装试了一下，感觉还不错，最起码查看log比较方便了。

#### Nginx

Nginx的前端代理配置

    # HTTP server
    #
    
    upstream node_com {
        server 127.0.0.1:8080;
        keepalive 8;
    }

    server {
        listen       0.0.0.0:80;
        server_name  www.domain.com;
    
        charset utf-8;
        access_log  /dir/log/log.log  main;

        location / {
            proxy_pass http://node_com;	
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $http_host;
            proxy_set_header X-NginX-Proxy true;
        
            proxy_redirect off;
        }
    }
    
#### Express

添加一行，说明其是在Nginx的后端，前面有一层代理。

	app.set('trust proxy', APP_RUNENV.TRUST);
	
	
#### 基于Forever的守护方式

在方便查找的位置放置一个autoload.sh：

    #!/bin/bash
    
    if [ $(ps -e -o uid,cmd | grep $UID | grep node | grep -v grep | wc -l | tr -s "\n") -eq 0 ]
    then
        export PATH=$PATH:/home/USERNAME/.nvm/v0.10.33/bin
        export NODE_PATH=$NODE_PATH:/home/USERNAME/.nvm/v0.10.33/lib/node_modules
        cd /home/USERNAME/domain.com && forever start ./autoload.js
        # 这里注意一下，直接填写绝对路径运行的结果和cd 然后运行的结果不同
    fi
    
Crontab配置：

	crontab -l
	# 查看任务列表
	
	crontab -e
	# 进入编辑模式，并添加一行：
	@reboot /home/USERNAME/domain.com/autoload.sh

#### 基于PM2的守护方式

先安装pm2吧，npm安装就是一句话：

	npm install -g pm2
	
然后是类似forver的启动方式：

	pm2 start autoload.js --watch
	
最后是pm2提供了一个简单的启动脚本的设置，使用比较方便。

	$ pm2 startup
	# auto-detect platform
	
	$ pm2 startup [platform]
	# render startup-script for a specific platform, the [platform] could be one of:
	#   ubuntu|centos|redhat|gentoo|systemd|darwin
	
这样貌似就成了，具体需要还是参考文档吧。

[PM2 - ADVANCED_README](https://github.com/Unitech/PM2/blob/development/ADVANCED_README.md)

今天再次测试的时候发现，pm2的startup并不靠谱，所以还是需要自己做一些东西，pm2启动以后基本是没什么问题的，但是自动启动的时候，需要一些环境变量什么的设置，因为我们是使用的nvm安装的，所以在启动脚本那边就这么写吧~

    #!/bin/bash
    # Active Nvm
    . ~/.nvm/nvm.sh
    # Export PATH & NODE_PATH
    # export PATH=$PATH:/home/{username}/.nvm/versions/node/v0.12.7
    # export NODE_PATH=$NODE_PATH:/home/{username}/.nvm/versions/node/v0.12.7/lib/node_modules
    # Active PM2
    cd /home/{username}/workspace/{project} && npm run pm2sp
