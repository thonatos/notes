
conf nginx with rtmp module

#### Command

    #
    # Install Lib
        sudo yum install pcre pcre-devel openssl openssl-devel zlib zlib-devel -y
    #
    # Download nginx && rtmp-module
        cd ~/download/
        wget http://nginx.org/download/nginx-1.8.0.tar.gz
        wget https://github.com/arut/nginx-rtmp-module/archive/v1.1.7.zip
    #
    # Extract zip
        tar -xvf nginx-1.8.0.tar.gz
        tar -xvf v1.1.7.zip
    #
    # Configure
        cd nginx-1.8.0
        ./configure --add-module=../nginx-rtmp-module-1.1.7/
        make
    #
    # Install
        sudo make install 

#### Systemd

    #
    # Add systemd 
    # /usr/lib/systemd/system/nginx.service
        [Unit]
        Description=nginx - high performance web server
        Documentation=http://nginx.org/en/docs/
        After=network.target remote-fs.target nss-lookup.target
        [Service]
        Type=forking
        PIDFile=/run/nginx.pid
        ExecStartPre=/usr/local/nginx/sbin/nginx -t -c /usr/local/nginx/conf/nginx.conf
        ExecStart=/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
        ExecReload=/bin/kill -s HUP $MAINPID
        ExecStop=/bin/kill -s QUIT $MAINPID
        PrivateTmp=true
        [Install]
        WantedBy=multi-user.target
    # 
    # Autoload
        systemctl enable nginx.service

#### Config

Add rtmp{} Before http{};

    rtmp_auto_push on;
    rtmp {
            server {
                    listen 1935;
                    chunk_size 4096;
                    timeout 10s;
                    # application record {
                    #        live on;
                    #        record all;
                    #        record_path /Users/thonatos/workspace/localhost_cdndl/local_assets/flv/;
                    #        # record_max_size 1M;
                    #}
                    application hls {
                            live on;
                            hls on;
                            hls_path /tmp/hls;
                            hls_fragment 5s;
                            # on_publish http://localhost:8035/event/status;
                            # on_done http://localhost:8035/event/status;
                            # on_publish_done http://localhost:8035/event/status;
                    }
            }
    }

Add server in http{};

	server {
	    listen      8000;
    location /hls {
        # Serve HLS fragments
        types {
            application/vnd.apple.mpegurl m3u8;
            video/mp2t ts;
        }
        root /tmp;
        add_header Cache-Control no-cache;
    }
	}

#### Usage

send steam & play.

    ## Send
    ffmpeg -re -i input.mp4 -c copy -f flv rtmp://192.168.3.8/hls/live
    ## Play
    ffplay http://192.168.1.122:8080/hls/live.m3u8