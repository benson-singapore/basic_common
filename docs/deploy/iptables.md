# 配置 iptables 防火墙

> 以下程序安装都是基于CentOS版本。<br>
由于centos7默认是使用firewall作为防火墙，下面介绍如何将系统的防火墙设置为iptables。

* [docker 安装](/deploy/install?id=docker-安装)

##### 配置 iptables

1. 停止firewall

``` bash 
systemctl stop firewall.service
```

2. 禁止firewall开机启动

``` vim 
systemctl disable firewall.service
```

3. 安装iptables

``` shell 
yum install iptables-services
```

4. 编辑防火墙文件

``` vim
vi /etc/sysconfig/iptables
# 添加80和3306端口
-A INPUT -m state –state NEW -m tcp -p tcp –dport 80 -j ACCEPT
-A INPUT -m state –state NEW -m tcp -p tcp –dport 3306 -j ACCEPT
```

5. 重启防火墙使配置文件生效

``` shell 
systemctl restart iptables.service
```

6. 设置iptables防火墙为开机启动项

``` shell 
systemctl enable iptables.service
```
