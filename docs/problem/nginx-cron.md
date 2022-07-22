# Nginx 跨域访问配置

<img src="./images/NGINX-logo-rgb-large.png" style="width:200px;float: right" class="no-zoom" />

> 以下程序都是基于CentOS版本。

* [跨域访问](/problem/nginx-cron?id=跨域访问)

##### 跨域访问

``` vim
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
  worker_connections 1024;
}

http {
  log_format main '$remote_addr - $remote_user [$time_local] "$request" '
  '$status $body_bytes_sent "$http_referer" '
  '"$http_user_agent" "$http_x_forwarded_for"';

  access_log /var/log/nginx/access.log main;

  sendfile on;
  tcp_nopush on;
  tcp_nodelay on;
  keepalive_timeout 65;
  types_hash_max_size 2048;

  client_max_body_size 50M;
  client_body_buffer_size 128k;
  fastcgi_intercept_errors on;

  include /etc/nginx/mime.types;
  default_type application/octet-stream;


  # Enable Gzip
  gzip off;
  gzip_http_version 1.0;
  gzip_comp_level 2;
  gzip_min_length 1100;
  gzip_buffers 4 8k;
  gzip_proxied any;
  gzip_types
  # text/html is always compressed by HttpGzipModule
  text/css
  text/javascript
  text/xml
  text/plain
  text/x-component
  application/javascript
  application/json
  application/xml
  application/rss+xml
  font/truetype
  font/opentype
  application/vnd.ms-fontobject
  image/svg+xml;

  gzip_static on;

  gzip_proxied expired no-cache no-store private auth;
  gzip_disable "MSIE [1-6]\.";
  gzip_vary on;

  include /etc/nginx/conf.d/*.conf;

  server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name _;
    root /usr/share/nginx/html/dist;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location ~ ^/(assets|images|javascripts|stylesheets|swfs|system)/ {
      gzip_static on;
      expires max;
    }

    location / {
      try_files $uri /index.html;
    }

    # Avoid CORS and reverse proxy settings
    location /api/ {
      proxy_http_version 1.1;
      proxy_pass http://127.0.0.1:8080/; # [3]

      add_header Access-Control-Allow-Origin *;
      add_header Access-Control-Allow-Methods "POST, GET, OPTIONS";
      add_header Access-Control-Allow-Headers "Origin, Authorization, Accept";
      add_header Access-Control-Allow-Credentials true;

      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $remote_addr;
      proxy_set_header X-Forwarded-Host $remote_addr;
    }

    location /image/{
      root /usr/share/nginx/html/upload;
    }

    location /video/{
      root /usr/share/nginx/html/upload;
    }
  }
```
