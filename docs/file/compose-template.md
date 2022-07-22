# docker-compose 模板

<img src="./images/docker-compose.png" style="width:200px;float: right" class="no-zoom" />

> 以下程序都是基于CentOS版本。

* [模板文件](/file/compose-template?id=模板文件)
* [安装redis](/file/compose-template?id=安装redis)
* [安装rabbitmq](/file/compose-template?id=安装rabbitmq)
* [安装mysql](/file/compose-template?id=安装mysql)

##### 模板文件

``` yaml
version: '3'
services:
  nginx:
    image: nginx
    container_name: nginx
    volumes:
      - ./nginx/html:/usr/share/nginx/html
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./nginx/logs:/var/log/nginx
    ports:
      - "80:80"
      - "443:443"
    command: [nginx, '-g', 'daemon off;']

  # api服务
  ant_api:
    image: ascdc/jdk8
    ports:
      - 8010:8010
    restart: 'always'
    container_name: ant_api
    environment:
      TZ: Asia/Shanghai
    volumes:
      - ./ant_api:/data/
    entrypoint: java -jar /data/webapps/ant_api.jar
```

!> nginx 安装可能会报错，如果报错的话，先注释掉 - ./nginx/nginx.conf:/etc/nginx/nginx.conf。然后cp 镜像内的nginx.conf到服务器的本地目录，然后再次运行docker-compose

##### 安装redis 

``` yaml
redis:
    image: redis
    container_name: redis
    restart: always
    command: --appendonly yes --requirepass 123456
    ports:
      - 6379:6379
    volumes:
      - ./redis_data:/data
```

> --requirepass 设置密码

##### 安装rabbitmq

``` yaml
rabbitmq:
    image: rabbitmq:management
    container_name: rabbit
    restart: always
    ports:
      - "15672:15672"
      - "5672:5672"
    environment:
      RABBITMQ_DEFAULT_USER=user
      RABBITMQ_DEFAULT_PASS=password
```

##### 安装mysql 

``` yaml
  mysql:
    image: mysql
    container_name: mysql
    ports:
      - 3306:3306
    volumes:
      - ./mysql_new/old_data:/var/lib/mysql
      - ./mysql_new/initdb:/docker-entrypoint-initdb.d
      - ./mysql_new/cnf/my.cnf:/etc/mysql/my.cnf
    command: [
      '--character-set-server=utf8mb4',
      '--collation-server=utf8mb4_unicode_ci',
      '--max_connections=3000'
    ]
    environment:
      TZ: Asia/Shanghai
      MYSQL_ROOT_PASSWORD: 123456
```
