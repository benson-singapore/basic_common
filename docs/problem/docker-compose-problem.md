# Docker 日志文件磁盘占满处理

<img src="./images/Docker.png" style="width:120px;float: right" class="no-zoom" />

> 以下程序都是基于CentOS版本。

* [Linux服务器因docker文件占满，导致无法启动问题。](/problem/docker-compose-problem?id=_1-linux服务器因docker文件占满，导致无法启动问题。)

##### 1. Linux服务器因docker文件占满，导致无法启动问题。

1. 对 /var/lib/docker/containers 下的文件夹进行排序，看看哪个容器占用了太多的磁盘空间

``` bash 
du -d1 -h /var/lib/docker/containers | sort -h
```

2. 选择你要清理的容器进行清理
   
``` bash 
cat /dev/null > /var/lib/docker/containers/container_id/container_log_name

# 例：
cat /dev/null > /var/lib/docker/containers/374aa0ba92b37d829012282ff15c1bb838d95dedb54589874c4285991be2d4aa/374aa0ba92b37d829012282ff15c1bb838d95dedb54589874c4285991be2d4aa-json.log
```

