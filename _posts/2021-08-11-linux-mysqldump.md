---
layout: post
title: "Linux下自动备份MySql数据库"
description: ""
category: "MySql"
tags: ["Linux","mysqldump","crontab","bluehost"]
---
{% include JB/setup %}


### mysqldump
mysqldump 是MySql自带的备份工具。 
```
<path_to_mysqldump>/mysqldump -h'host' -p'port' -u'username' -p'password' 'dbnmae' > backup.sql
```
host, port可选。       
可以用gzip压缩
```
<path_to_mysqldump>/mysqldump -h'host' -p'port' -u'username' -p'password' 'dbnmae' | gzip > backup.sql.gz
```
----
   
### 可将mysqldump写在sh脚本文件里将备份文件命名变量化
```
#!/bin/sh
DATE=`date +%Y%m%d_%H%M%S`
mysqldump --opt -u'username' -p'password' 'dbnmae' | gzip > dump_$DATE.sql.gz
```
----
   
### crontab 是linux定时任务机制
```
crontab -e //进入编辑模式
```
任务格式
```
0 0 * * * <path to sh>/xxx.sh 
```
* 前面5个部分是关于时间的配置

   * 第一个表示分钟，这里表示0分时刻
   * 第二个表示小时，这里写0表示0时刻，也就是每天晚上0点时刻
   * 第三个表示每个月份中的第几日
   * 第四个表示一年中的月份
   * 第五个表示每个星期中的第几天

   * 当以上时间数字为具体数字的时候，则表示第多少时刻执行
   * 当以上时间数字为*的时候，则表示所有时刻执行
   * 当以上时间数字为n/2的时候，则表示每隔时间执行

* xxx.sh为执行的脚本。    

* 可在后面加上 >/dev/null 2>&1 &
```
0 0 * * * <path to sh>/xxx.sh >/dev/null 2>&1 &
```
   * >/dev/null
      * 这条命令的作用是将标准输出1重定向到/dev/null中。 /dev/null代表linux的空设备文件，所有往这个文件里面写入的内容都会丢失，俗称“黑洞”。那么执行了>/dev/null之后，标准输出就会不再存在，没有任何地方能够找到输出的内容。
   * 2>&1。
      * 这条命令用到了重定向绑定，采用&可以将两个输出绑定在一起。这条命令的作用是错误输出将和标准输出同用一个文件描述符，说人话就是错误输出将会和标准输出输出到同一个地方。
      * linux在执行shell命令之前，就会确定好所有的输入输出位置，并且从左到右依次执行重定向的命令，所以>/dev/null 2>&1的作用就是让标准输出重定向到/dev/null中（丢弃标准输出），然后错误输出由于重用了标准输出的描述符，所以错误输出也被定向到了/dev/null中，错误输出同样也被丢弃了。执行了这条命令之后，该条shell命令将不会输出任何信息到控制台，也不会有任何信息输出到文件中。

   * 末尾的 &
      * Linux 终端命令的末尾加上一个 & 符号表示将这个任务放到后台去执行。 对于需要长时间执行的任务，&符号可以使当前终端窗口不被占用，这样就能继续在同一个终端上工作，甚至关闭终端窗口也不会影响任务的正常执行。不过需要注意的是，如果要求这个任务输出内容到标准输出中（例如 echo 或 ls），即便使用了 &，也会等待这些输出任务在前台运行完毕;   

----
    
### 在[Bluehost](https://www.bluehost.com/)中设置计划任务。
在用户中心cPanel下找到**Cron Jobs** (在Advanced下)。时间设置无难度，关键是command的设置。 
#### Command
* MySql: "path_to_sql"要在从/home开始
   * Command to import a database:
   ```
   mysql -umysql_user -ppassword database_name < path_to_sql/backup.sql
   ```
   * Command to export a database:
   ```
   mysqldump -umysql_user -ppassword database_name > path_to_sql/backup.sql
   ```

* SSH: "path_to_sh"要在从/home开始
   * Command to run a shell script cron job:
   ```
   /bin/sh path_to_sh/file.sh 
   ```
----
    
参考文章链接：
* [https://www.difashi.com/2020-01/01-mysql-mysqldump.html](https://www.difashi.com/2020-01/01-mysql-mysqldump.html)   
* [https://www.difashi.com/2019-11/14-linux-crontab.html](https://www.difashi.com/2019-11/14-linux-crontab.html)
* [https://www.cnblogs.com/ultranms/p/9353157.html](https://www.cnblogs.com/ultranms/p/9353157.html)
* [https://blog.csdn.net/willingtolove/article/details/113933488](https://blog.csdn.net/willingtolove/article/details/113933488)
* [https://www.bluehost.com/hosting/help/168](https://www.bluehost.com/hosting/help/168)