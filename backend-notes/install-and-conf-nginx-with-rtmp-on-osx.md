
conf nginx with rtmp module

#### Command

	brew tap homebrew/nginx
	brew options nginx-full
	brew info nginx-full
	brew install nginx-full --with-rtmp-module --with-debug

#### Config

Add rtmp{} Before http{};

    rtmp_auto_push on;
    rtmp {
            server {
                    listen 1935;
                    chunk_size 400000;
                    timeout 10s;
                    application live {
                            live on;
                            record all;
                            record_path /tmp;
                            record_max_size 1M;
                    }
                    application hls {
                            live on;
                            hls on;
                            hls_path /tmp/hls;
                            hls_fragment 5s;
                    }
            }
    }

Add server in http{};

	server {
	    listen      8000;
	    # This URL provides RTMP statistics in XML
	    location /stat {
	        rtmp_stat all;
	        # Use this stylesheet to view XML as web page
	        # in browser
	        rtmp_stat_stylesheet stat.xsl;
	    }
	    location /stat.xsl {
	        # XML stylesheet to view RTMP stats.
	        # Copy stat.xsl wherever you want
	        # and put the full directory path here
	        root /usr/local/etc/nginx/rtmp/stat.xsl/;
	    }
	    location /hls {
	        # Serve HLS fragments
	        types {
	            application/vnd.apple.mpegurl m3u8;
	            video/mp2t ts;
	        }
	        root /tmp;
	        add_header Cache-Control no-cache;
	    }
	    location /dash {
	        # Serve DASH fragments
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