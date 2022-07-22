# 安装

<img src="./images/Docker.png" style="width:150px;float: right" class="no-zoom" />

> 以下程序安装都是基于CentOS版本。

* [docker 安装](/deploy/install?id=docker-安装)
* [docker-compose 安装](/deploy/install?id=docker-compose-安装)

##### docker 安装

1. 卸载旧版本

```bash
sudo yum remove docker \
              docker-client \
              docker-client-latest \
              docker-common \
              docker-latest \
              docker-latest-logrotate \
              docker-logrotate \
              docker-engine
```

2. 安装 Docker Engine-Community

设置仓库
安装所需的软件包。yum-utils 提供了 yum-config-manager ，并且 device mapper 存储驱动程序需要 device-mapper-persistent-data 和 lvm2。

``` bash
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
```

使用以下命令来设置稳定的仓库。

``` bash
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

安装最新版本的 Docker Engine-Community 和 containerd，或者转到下一步安装特定版本：

``` bash
sudo yum install docker-ce docker-ce-cli containerd.io
```

##### docker-compose 安装

1. 运行以下命令以下载 Docker Compose 的当前稳定版本：

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2. 将可执行权限应用于二进制文件：

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

3. 创建软链：

```bash
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

4. 测试是否安装成功：

``` bash
docker-compose --version

docker-compose version 1.25.0, build 1110ad01
```
