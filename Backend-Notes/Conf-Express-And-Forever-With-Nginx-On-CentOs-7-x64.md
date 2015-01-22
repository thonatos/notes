Express、ForeverJs、Nginx On CentOs-7.x

文档站基于ExpressJs跑在Vps，使用了Nginx做前端代理，转发请求到内部网络的NodeJs服务器上，同时为了能让他自己启动，安装了ForeverJs做进程守护，保证其发生问题后自动重启，同时考虑到机器重启后需要自启动ForeverJS，又使用了Crontab作为计划任务。

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
    
#### ExpressJs

添加一行，说明其是在Nginx的后端，前面有一层代理。

	app.set('trust proxy', APP_RUNENV.TRUST);
	
	
#### Autoload.sh

这里注意一下，直接填写绝对路径运行的结果和cd 然后运行的结果不同。

    #!/bin/bash
    
    if [ $(ps -e -o uid,cmd | grep $UID | grep node | grep -v grep | wc -l | tr -s "\n") -eq 0 ]
    then
        export PATH=$PATH:/home/USERNAME/.nvm/v0.10.33/bin
        export NODE_PATH=$NODE_PATH:/home/USERNAME/.nvm/v0.10.33/lib/node_modules
        cd /home/USERNAME/domain.com && forever start ./autoload.js
    fi
    
#### Crontab

	crontab -l
	# 查看任务列表
	
	crontab -e
	# 进入编辑模式，并添加一行：
	@reboot /home/USERNAME/domain.com/autoload.sh


