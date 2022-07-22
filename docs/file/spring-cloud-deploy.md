# Spring Cloud 部署配置

<img src="./images/spring-cloud (1).png" style="width:330px;float: right;margin-top: -40px" class="no-zoom" />

> 以下程序都是基于CentOS版本。

* [配置信息](/cmd/docker-compose-cmd?id=命令)

##### docker-compose 项目部署

``` yaml
version: '3'
services:
  #nginx
  nginx:
    image: nginx
    container_name: nginx
    volumes:
      - ./nginx/html:/usr/share/nginx/html
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./nginx/logs:/var/log/nginx
    ports:
      - "80:80"
      - "8080:8080"
      - "443:443"
    command: [nginx, '-g', 'daemon off;']
  # eureka
  eureka-server:
    image: ascdc/jdk8
    ports:
      - 8761:8761
    container_name: eureka-server
    environment:
      TZ: Asia/Shanghai
    logging:
      driver: "json-file"
      options:
        max-size: "5g"
    volumes:
      - ./springcloud:/data/
    entrypoint: java -jar /data/webapps/eureka-server.jar
    tty: true
  # config
  config-server:
    image: ascdc/jdk8
    ports:
      - 8000:8000
    container_name: config-server
    environment:
      TZ: Asia/Shanghai
      spring.cloud.config.server.native.search-locations: classpath:/test
    logging:
      driver: "json-file"
      options:
        max-size: "5g"
    volumes:
      - ./springcloud:/data/
    entrypoint: java -jar /data/webapps/config-server.jar
    tty: true
  # 授权
  uaa-service:
    image: ascdc/jdk8
    ports:
      - 8010:8010
    container_name: uaa-service
    environment:
      TZ: Asia/Shanghai
      spring.cloud.config.uri: http://****:****@**.**.**.**:8000
      eureka.client.serviceUrl.defaultZone: http://****:****@**.**.**.**:8761
      eureka.instance.instance-id: http://**.**.**.**:8010
      eureka.instance.ip-address: **.**.**.**
    logging:
      driver: "json-file"
      options:
        max-size: "5g"
    volumes:
      - ./springcloud:/data/
    entrypoint: java -jar /data/webapps/uaa-service.jar
    tty: true
  # 用户
  user-service:
    image: ascdc/jdk8
    ports:
      - 8030:8030
    container_name: user-service
    environment:
      TZ: Asia/Shanghai
      spring.cloud.config.uri: http://****:****@**.**.**.**:8000
      eureka.client.serviceUrl.defaultZone: http://****:****@**.**.**.**:8761
      eureka.instance.instance-id: http://**.**.**.**:8030
      eureka.instance.ip-address: **.**.**.**
    logging:
      driver: "json-file"
      options:
        max-size: "5g"
    volumes:
      - ./springcloud:/data/
      - ./nginx/html/upload:/upload
    entrypoint: java -jar /data/webapps/user-service.jar
    tty: true
  # 网站
  web-service:
    image: ascdc/jdk8
    ports:
      - 8060:8060
    container_name: web-service
    environment:
      TZ: Asia/Shanghai
      spring.cloud.config.uri: http://****:****@**.**.**.**:8000
      eureka.client.serviceUrl.defaultZone: http://****:****@**.**.**.**:8761
      eureka.instance.instance-id: http://**.**.**.**:8060
      eureka.instance.ip-address: **.**.**.**
    logging:
      driver: "json-file"
      options:
        max-size: "5g"
    volumes:
      - ./springcloud:/data/
    entrypoint: java -jar /data/webapps/web-service.jar
    tty: true
  # 日志
  log-service:
    image: ascdc/jdk8
    ports:
      - 8050:8050
    container_name: log-service
    environment:
      TZ: Asia/Shanghai
      spring.cloud.config.uri: http://****:****@**.**.**.**:8000
      eureka.client.serviceUrl.defaultZone: http://****:****@**.**.**.**:8761
      eureka.instance.instance-id: http://**.**.**.**:8050
      eureka.instance.ip-address: **.**.**.**
    logging:
      driver: "json-file"
      options:
        max-size: "5g"
    volumes:
      - ./springcloud:/data/
    entrypoint: java -jar /data/webapps/log-service.jar
    tty: true
  # Grasp
  grasp-service:
    image: ascdc/jdk8
    ports:
      - 8040:8040
#      - 8110:8110
    container_name: grasp-service
    environment:
      TZ: Asia/Shanghai
      spring.cloud.config.uri: http://****:****@**.**.**.**:8000
      eureka.client.serviceUrl.defaultZone: http://****:****@**.**.**.**:8761
      eureka.instance.instance-id: http://**.**.**.**:8040
      eureka.instance.ip-address: **.**.**.**
    logging:
      driver: "json-file"
      options:
        max-size: "5g"
    volumes:
      - ./springcloud:/data/
    # 远程调试
#    entrypoint: java -Xdebug -Xrunjdwp:transport=dt_socket,address=8110,server=y,suspend=n -jar /data/webapps/lottery-grasp.jar
    entrypoint: java -jar /data/webapps/grasp-service.jar
    tty: true
  # 网关
  gateway-service:
    image: ascdc/jdk8
    ports:
      - 8020:8020
    container_name: gateway-service
    environment:
      TZ: Asia/Shanghai
      spring.cloud.config.uri: http://****:****@**.**.**.**:8000
      eureka.client.serviceUrl.defaultZone: http://****:****@**.**.**.**:8761
      eureka.instance.instance-id: http://**.**.**.**:8020
      eureka.instance.ip-address: **.**.**.**
    logging:
      driver: "json-file"
      options:
        max-size: "5g"
    volumes:
      - ./springcloud:/data/
    entrypoint: java -jar /data/webapps/gateway-service.jar
    tty: true
```
