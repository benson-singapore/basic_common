# nginx 静态文件配置

<img src="./images/NGINX-logo-rgb-large.png" style="width:200px;float: right" class="no-zoom" />

> 以下程序都是基于CentOS版本。

* [配置信息](/cmd/docker-compose-cmd?id=命令)

##### 配置信息

``` vim
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    gzip  on;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    server {
        listen 80;
        # gzip config
        gzip on;
        gzip_min_length 1k;
        gzip_comp_level 9;
        gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
        gzip_vary on;
        gzip_disable "MSIE [1-6]\.";

        location / {
            root   /usr/share/nginx/html/dist;
            index  index.html index.htm;

            # 用于配合 browserHistory使用
            try_files $uri $uri/ /index.html;
        }
    }
}
```
