# docker-compose 命令

<img src="./images/docker-compose.png" style="width:200px;float: right" class="no-zoom" />

> 以下程序都是基于CentOS版本。

* [命令](/cmd/docker-compose-cmd?id=命令)

##### 命令

?> 如果对docker compose 具体配置有问题可参考官方文档[docker-compose](https://docs.docker.com/compose/compose-file/)

``` shell
### 查看帮助
docker-compose -h

# -f  指定使用的 Compose 模板文件
# 默认为 docker-compose.yml，可以多次指定。
docker-compose -f docker-compose.yml up -d 

#启动所有容器，-d 将会在后台启动并运行所有的容器
docker-compose up -d

#停用移除所有容器以及网络相关
docker-compose down

#查看服务容器的输出
docker-compose logs

#列出项目中目前的所有容器
docker-compose ps

#构建（重新构建）项目中的服务容器。服务容器一旦构建后，将会带上一个标记名.
#例如对于 web 项目中的一个 db 容器，可能是 web_db。
#可以随时在项目目录下运行 docker-compose build 来重新构建服务
docker-compose build

# 不带缓存的构建。
docker-compose build --no-cache

#拉取服务依赖的镜像
docker-compose pull

#重启项目中的服务
docker-compose restart

#删除所有（停止状态的）服务容器。
#推荐先执行 docker-compose stop 命令来停止容器。
docker-compose rm 

#在指定服务上执行一个命令。
docker-compose run ubuntu ping docker.com

#设置指定服务运行的容器个数。通过 service=num 的参数来设置数量
docker-compose scale web=3 db=2

#启动已经存在的服务容器。
docker-compose start

#停止已经处于运行状态的容器，但不删除它。
#通过 docker-compose start 可以再次启动这些容器。
docker-compose stop
```
