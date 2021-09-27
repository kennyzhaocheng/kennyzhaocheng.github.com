---
layout: post
title: "Linux下自动备份mongodb数据库"
description: ""
category: "MongoDB"
tags: ["Linux","mongodbdump","crontab","MongoDB"]
---
{% include JB/setup %}


### 创建备份目录：
```
mkdir -p /data/backup/mongo/mongodb_bak_tmp
mkdir -p /data/backup/mongo/mongodb_bak_path
```

### 脚本如下，命名为：/mongodbfullbackup.sh：
```
#!/bin/bash
#mongodump命令路径
DUMP=/usr/local/mongoDB/mongodbserver/bin/mongodump
#tar备份包临时备份目录,这个tar包最好定时传到本地存储做备份
OUT_DIR=/data/backup/mongo/mongodb_bak_tmp
#完整备份目录路径
TAR_DIR=/data/backup/mongo/mongodb_bak_path
#获取当前系统时间
DATE=`date +%Y_%m_%d_%H_%M`
#数据库账号
DB_USER=kennyb
#数据库密码
DB_PASS=xxxxxxxx
#DAYS=15代表删除15天前的备份，即只保留近15天的备份
DAYS=15
#最终保存的数据库备份文件
TAR_BAK="mongodb_bak_$DATE.tar.gz"

cd $OUT_DIR
#rm -rf $OUT_DIR/*
mkdir -p $OUT_DIR/$DATE
#备份全部数据库
$DUMP -h <host>:<port> -u $DB_USER -p $DB_PASS --authenticationDatabase "db_name" -o $OUT_DIR/$DATE
#压缩为.tar.gz格式
tar -zcvf $TAR_DIR/$TAR_BAK $OUT_DIR/$DATE
#删除15天前的备份文件
find $TAR_DIR/ -mtime +$DAYS -delete
#删除tar备份包10天前的备份文件
find $OUT_DIR/ -mtime +10 -name "*.tar.gz" -exec rm -rf {} \;
exit

```

### 设置定时任务：
```
 crontab -e
```

设置时间：
shell脚本文件路径要实用绝对路径，即从根目录开始。
```
0 3 * * * /data/script/mongodbfullbackup.sh >/dev/null 2>&1 &
```

### 查看定时任务实时log
```
tail -f /var/log/cron
```

### 我遇到错误：(root) FAILED to authorize user with PAM (Module is unknown)
解决办法：
打开 /etc/pam.d/crond， 把
```
session    required   pam_loginuid.so 
```
修改为：
```
session    sufficient   pam_loginuid.so
```
重启crond
```
service crond stop
service crond start
service crond status
```
