---
layout: post
title: 懒人怎么从远程linux主机拷贝文件
date: 2016-10-28T00:00:00+00:00
categories: linux
lang: zh
---
怎样能够快速的从服务器拷贝一个文件或者目录？

如果你想从linux机器上考一个文件过来，
又不想搭一个ftp服务器，也不想做文件共享怎么办？

# scp命令是个很好的选择

### 拷贝目录：
```
scp -r root@ip :/alidata/server/ovo_jar/ .
```
### 拷贝文件
```
scp root@ip:/alidata/server/start_hbase-thrift_all.log start_hbase-thrift_all.log
```

作为一个懒人，通常会配置个免密码登录
<!-- more -->
### 配置免密码登录

参考[CentOS 配置SSH免密码登陆](http://www.centoscn.com/CentOS/config/2014/0611/3125.html)

  ```
cat ~/.ssh/id_rsa.pub | ssh root@ip "cat - >> ~/.ssh/authorized_keys"
  ```
# 2. 作为一个超级懒人,还会忍不住要写个脚本

```
#!/bin/sh

file_path=$1

file_name=`basename ${file_path}`
dir_name=`dirname ${file_path}`
echo "copying ${file_name} ..."
echo "from ovo_web:${dir_name} to current dir"

if ssh ovo_web test -d $file_path;
    then scp -r root@ovo_web:${file_path} .
    else scp root@ovo_web:${file_path} ${file_name}
fi
```
注释：这里的ovo_web 是我的服务器ip，
懒到家的我在hosts里面吧这个ip配置成了ovo_web

# 此后,拷贝文件就变成了这样
拷贝目录
```
scp_from_ovo_web.sh "/alidata/server/ovo_jars"
```
拷贝文件
```
scp_from_ovo_web.sh "/alidata/server/start_hbase-thrift_all.log"
```
