---
layout: post
title: "CentOS6.9 安装Node和MongoDB"
description: ""
category: "Server"
tags: ["Linux","centOS","nvm","Node",'MongoDB']
---
{% include JB/setup %}

我用的是腾讯云的centOS6.9的服务器。

### 安装NVM

NVM的github地址：https://github.com/nvm-sh/nvm。
我一开始用下面官方的script安装都没有成功：
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```
or
```
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```
后来我尝试manual install, 因为没有安装git,也没有成功。

先安装git
```
yum install git
```
再安装nvm
```
export NVM_DIR="$HOME/.nvm" && (
  git clone https://github.com/nvm-sh/nvm.git "$NVM_DIR"
  cd "$NVM_DIR"
  git checkout `git describe --abbrev=0 --tags --match "v[0-9]*" $(git rev-list --tags --max-count=1)`
) && \. "$NVM_DIR/nvm.sh"
```
把下面加入~/.bashrc, ~/.profile, or ~/.zshrc (通过vim编辑，或者WinSCP)
```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```
----

### 安装Node
```
nvm install node
```

检查node的版本, 看到版本号就说明node安装成功了。
```
node --version
```
但是，我遇到了几个错误：
* 第一个错误： 
```
/lib64/libc.so.6: version `GLIBC_2.17' not found 
```
需要手动下载2.17版本的glibc库。 所有版本在：https://ftp.gnu.org/gnu/glibc/。 2.17的版本在：https://ftp.gnu.org/gnu/glibc/glibc-2.17.tar.gz。
下载并安装：
```
wget https://ftp.gnu.org/gnu/glibc/glibc-2.17.tar.gz
tar -xvf glibc-2.17.tar.gz
```
编译安装:
```
cd glibc-2.17
mkdir build
cd build
../configure --prefix=/usr --disable-profile --enable-add-ons --with-headers=/usr/include --with-binutils=/usr/bin
make && make install
```
查看共享库：
```
ls -l /lib64/libc.so.6
```
再次查看系统中可使用的glibc版本,会发现已更新至2.17版本
```
strings /lib64/libc.so.6 |grep GLIBC_
```

* 第二个错误：
```
configure: error: no acceptable C compiler found in $PATH
```
解决办法如下：安装GCC软件套件
```
yum install gcc
```

* 第三个错误：
```
 /usr/lib64/libstdc++.so.6: version `GLIBCXX_3.4.21' not found
```
这是因为升级gcc时，生成的动态库没有替换老版本gcc的动态库导致的，将gcc最新版本的动态库替换系统中老版本的动态库即可解决。

运行以下命令检查动态库：
```
find / -name "libstdc++.so*"
```
将检查到的最新动态库复制到/usr/lib64目录下：
```
cp /home/gcc-5.2.0/gcc-temp/stage1-x86_64-unknown-linux-gnu/libstdc++-v3/src/.libs/libstdc++.so.6.0.21 /usr/lib64
```
修改系统默认动态库的指向，即:重建默认库的软链接.
```
cd /usr/lib64
rm -rf libstdc++.so.6
ln -s libstdc++.so.6.0.21 libstdc++.so.6
```
重新运行以下命令检查动态库, 会发现已更新至最新。
```
strings /usr/lib64/libstdc++.so.6 | grep GLIBC
```

* 第四个错误：缺少共享库
```
error while loading shared libraries: libstdc++.so.6
```

查找缺少哪个共享库：
```
yum whatprovides libstdc++.so.6
```
根据查找出来的共享库来安装：
```
yum install libstdc++-4.4.7-18.el6_9.2.i686
```

----

### 安装MongoDB
 * 创建mongoDB文件夹：
```
cd /usr/local
mkdir mongoDB
```

* 到官网下载mongodb， 选择版本，例如：mongodb-linux-x86_64-rhel62-4.2.15.tgz
https://www.mongodb.com/try/download/community
https://www.mongodb.org/dl/linux
用WinSCP工具上传至Linux服务器目录：/usr/local/source

* 解压安装
```
cd /usr/local/source
tar -zxvf mongodb-linux-x86_64-rhel62-4.2.15.tgz -C /usr/local/mongoDB
cd /usr/local/mongoDB
mv mongodb-linux-x86_64-rhel62-4.2.15 mongodbserver
```

* 配置文件
```
cd /usr/local/mongoDB/mongodbserver
mkdir data
mkdir logs
mkdir etc
cd /usr/local/mongoDB/mongodbserver/etc
vim mongodb.conf
```
mongodb.conf文件内容如下：
```
dbpath=/usr/local/mongoDB/mongodbserver/data
logpath=/usr/local/mongoDB/mongodbserver/logs/mongodb.log
port=27017
fork=true
journal=false
bind_ip=0.0.0.0
```

* 启动mongoDB
```
cd /usr/local/mongoDB/mongodbserver/bin
./mongod --config /usr/local/mongoDB/mongodbserver/etc/mongodb.conf
```
访问http://server:27017/ , 可以看到response,即安装成功。

* 添加管理用户
```
cd /usr/local/mongoDB/mongodbserver/bin
./mongo
```
在mongo界面中：
```
use admin
db.createUser( {user: "kennyadmin",pwd: "123456",roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]});
show users
db.shutdownServer()
```
使用权限方式再次启动MongoDB
在配置文件mongodb.conf中添加：auth=true , 然后启动
```
cd /usr/local/mongoDB/mongodbserver/bin
./mongod --config /usr/local/mongoDB/mongodbserver/etc/mongodb.conf
./mongo
```
在mongo界面中：
```
use admin
db.auth("kennyadming","123456")  #认证，返回1表示成功
```

* 将mongod路径添加到系统路径中，方便随处执行mongod命令
在/etc/profile文件中，添加 export PATH=$PATH:/usr/local/mongoDB/mongodbserver/bin
执行source /etc/profile，使系统环境变量立即生效
```
source /etc/profile
```

* 将mongo路径软链到/usr/bin路径下，方便随处执行mongo命令
```
ln -s /usr/local/mongoDB/mongodbserver/bin/mongo  /usr/bin/mongo
```

* 测试是否方便随处执行mongo命令
回到任意路径下,执行mongo命令,连接mongod服务
```
mongo
```
在mongo界面中：
```
use admin
db.auth("kennyadming","123456")  
db.shutdownServer()
```
出现错误"not authorized on admin to execute command { shutdown: 1.0 }"
解决办法：
```
db.updateUser(
 "kennyadmin",
        {
           roles : [
                     {"role" : "userAdminAnyDatabase","db" : "admin"},
                     {"role" : "dbOwner","db" : "admin"},
                     {"role" : "clusterAdmin", "db": "admin"}
                   ]
        }
 )
```

* MongoDB设置为系统服务并且设置开机启动
首先添加MongoDB系统服务，命令如下：
```
vim /etc/rc.d/init.d/mongod
```
打开编辑器后，我们将下面的配置粘贴进去，然后保存
```
   start() {  
   /usr/local/mongoDB/mongodbserver/bin/mongod  --config /usr/local/mongoDB/mongodbserver/etc/mongodb.conf 
   }  
   
   stop() {  
   /usr/local/mongoDB/mongodbserver/bin/mongod --config /usr/local/mongoDB/mongodbserver/etc/mongodb.conf --shutdown  
   }  
   case "$1" in  
   start)  
   start  
   ;;  
   
   stop)  
   stop  
   ;;  
   
   restart)  
   stop  
   start  
   ;;  
   *)  
   echo  
   $"Usage: $0 {start|stop|restart}"  
   exit 1  
   esac
```
保存完成之后，添加脚本执行权限，命令如下：
```
chmod +x /etc/rc.d/init.d/mongod
```
启动MongoDB，
```
service mongod start
```
关闭MongoDB服务:
```
service mongod stop
```