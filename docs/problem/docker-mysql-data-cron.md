# Docker 定时备份mysql数据库

<img src="./images/1200px-MySQL.svg.png" style="width:180px;float: right" class="no-zoom" />

> 以下程序都是基于CentOS版本。

* [备份mysql数据库](/problem/docker-mysql-data-cron?id=备份mysql数据库)

##### 备份mysql数据库

1、 编写自动备份脚本 `mysql_backup.sh`

``` bash
#! /bin/bash

DATE=`date +%Y%m%d%H%M%S`
BACK_DATA=asean_lotto-${DATE}.sql

docker exec -it mysql  mysqldump asean_lotto -uroot -p123456 > /opt/data/mysql/${BACK_DATA}

gzip -c /opt/data/mysql/${BACK_DATA} > /opt/data/mysql/${BACK_DATA}.gz

rm /opt/data/mysql/${BACK_DATA}
```

2、编辑crond配置，每分钟执行下脚本：`vi /etc/crontab`

> 注： crond具体的使用方式，可以自行百度/google

``` vim
脚本内容如下：
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
# For details see man 4 crontabs
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed

1 0 * * * root /opt/data/mysql_backup.sh 
```

