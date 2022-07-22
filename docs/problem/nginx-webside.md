# Nginx 单服务多域名配置

<img src="./images/NGINX-logo-rgb-large.png" style="width:200px;float: right" class="no-zoom" />

> 以下程序都是基于CentOS版本。

* [多域名配置](/problem/nginx-webside?id=多域名配置)

##### 多域名配置

``` vim

  server {
    listen *:80;
    listen [::]:80;
    server_name www.api-tools.com;
    return 301 http://api-tools.com$request_uri;
  }

  server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name api-tools.com;
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

    location /image/{
      root /usr/share/nginx/html/upload;
    }

    location /video/{
      root /usr/share/nginx/html/upload;
    }
  }

###########

  server {
    listen *:80;
    listen [::]:80;
    server_name www.api-tools-doc.com;
    return 301 http://api-tools-doc.com$request_uri;
  }

  server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name api-tools-doc.com;
    root /usr/share/nginx/html/distdoc;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location ~ ^/(assets|images|javascripts|stylesheets|swfs|system)/ {
      gzip_static on;
      expires max;
    }

    location / {
      try_files $uri /index.html;
    }

    location /image/{
      root /usr/share/nginx/html/upload;
    }

    location /video/{
      root /usr/share/nginx/html/upload;
    }
  }

```
