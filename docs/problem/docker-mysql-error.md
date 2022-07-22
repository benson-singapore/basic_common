# Docker Mysql 如何找回数据库 

<img src="./images/1200px-MySQL.svg.png" style="width:180px;float: right" class="no-zoom" />

> 以下程序都是基于CentOS版本。

* [重新设置mysql映射目录](/problem/docker-mysql-error?id=重新设置mysql映射目录)

##### 重新设置mysql映射目录

1. docker mysql如果在容器异常后，找不到数据库。却又未设置映射目录的情况下，可依据如下情况处理

``` bash 
# 查找对应的容器映射目录
ls /var/lib/docker/volumes
```

2. 创建新的容易，并映射对应的数据库数据

``` yaml
version: '3'
services:
  # 数据库
  mysql_new:
    image: mysql
    container_name: mysql_new
    ports:
      - 3306:3306
    volumes:
      - /var/lib/docker/volumes/ab910d6224d19a53087213d2f011c377c1ec189a1714aea04b3312ee501a9a68/_data:/var/lib/mysql
    command: [
      '--character-set-server=utf8mb4',
      '--collation-server=utf8mb4_unicode_ci',
      '--max_connections=3000'
    ]
    environment:
      TZ: Asia/Shanghai
      MYSQL_ROOT_PASSWORD: 123456
```
