# Docker Java远程代码调试

<img src="./images/idea.png" style="width:120px;float: right; margin-left: 40px" class="no-zoom" />
<img src="./images/debug.png" style="width:180px;float: right" class="no-zoom" />

> 以下程序都是基于CentOS版本。<br>
> 项目部署都是基于 docker-compose。

* [SpringBoot 项目远程调试](/problem/docker-remote?id=springboot-项目远程调试)
* [Tomcat 项目远程调试](/problem/docker-remote?id=tomcat-项目远程调试)

##### SpringBoot 项目远程调试

1. 修改docker-compose文件，增加远程调试端口。

``` yaml
version: '3'
services:
  # api服务
  ant_api:
    image: ascdc/jdk8
    ports:
      - 8010:8010
      - 8110:8110
    restart: 'always'
    container_name: ant_api
    environment:
      TZ: Asia/Shanghai
    volumes:
      - ./ant_api:/data/
#    entrypoint: java -jar /data/webapps/ant_api.jar
    entrypoint: java -Xdebug -Xrunjdwp:transport=dt_socket,address=8110,server=y,suspend=n -jar /data/webapps/lottery-grasp.jar
```

2. Idea 配置远程调试端口。

<img src="./images/WX20200524-190214@2x.png" style="width:780px;" class="no-zoom" />


##### Tomcat 项目远程调试

1. 配置tomcat `/bin/catalina.sh`

``` vim
export JAVA_OPTS='-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8000'
```

2. Idea 配置远程调试端口。
