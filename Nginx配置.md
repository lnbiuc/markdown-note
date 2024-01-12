```nginx
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}
http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    include /etc/nginx/conf.d/*.conf;
	# 开启gzip，nginx会返回静态资源的压缩文件，减少文件体积，加快页面加载速度
    gzip on;
    gzip_static on;
	gzip_min_length 1k;
    gzip_comp_level 7;
    gzip_buffers 4 16k;
    gzip_http_version 1.1;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;
    gzip_vary on;

	# 将所有80端口的请求强制转发到443端口并且使用https://www
	server {
		listen 80 default_server;
		listen [::]:80 default_server;
		server_name beyondhorizon.top www.beyondhorizon.top;
		# 跨域配置
		add_header 'Access-Control-Allow-Origin' '*';
		add_header 'Access-Control-Allow-Headers' '*';
		add_header 'Access-Control-Allow-Methods' '*';
		# 无条件放行option请求
			if ($request_method = 'OPTIONS') {
				return 204;
			}
		# 返回307 而不是 304
		return 307 https://www.beyondhorizon.top$request_uri;
	}



    server {
        listen *:443 ssl http2;
        listen [::]:443 ssl http2;
        server_name beyondhorizon.top www.beyondhorizon.top;
		# ssl证书配置
        ssl_certificate beyondhorizon.top_bundle.crt;
        ssl_certificate_key beyondhorizon.top.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1.2 TLSv1.3;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers on;
		# 运行客户端上传文件的最大体积
        client_max_body_size 10m;

		# 优先过滤特殊的请求，返回项目后台管理界面
		location = /admin {
                root /nginx/html;
                index admin.html;
				# 跨域配置
                add_header Access-Control-Allow-Origin *;
                add_header Access-Control-Allow-Headers Origin,X-Requested-Width,Content-Type,Accept;
                add_header Access-Control-Allow-Methods GET,POST,PUT,DELETE,OPTIONS;
                try_files $uri $uri/ /admin.html;
        }
		
		# 其他所有请求返回首页
        location / {
                root /nginx/html;
                index index.html;
                try_files $uri $uri/ /index.html;
        }
		# 请求api转发到springboot进行处理
        location ~ /api/ {
                proxy_pass http://127.0.0.1:8888;
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }
}
```

