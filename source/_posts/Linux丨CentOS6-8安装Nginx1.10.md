---
title: Linux丨CentOS6.8安装Nginx1.10
date: 2019-03-01 11:02:51
categories: "Linux"
tags: 
  - Linux
  - Nginx
---

### 安装环境
```
- 系统环境：CentOS6.8 64bit
- JDK版本：1.10.2
```

### 安装依赖
```shell
yum -y install gcc zlib zlib-devel pcre-devel openssl openssl-devel
```


### 下载`nginx`
```shell
wget http://nginx.org/download/nginx-1.10.2.tar.gz
```


### 解压
```shell
tar -zxvf nginx-1.10.2.tar.gz
```

### 执行配置文件
```shell
cd nginx-1.10.2
./configure
```

### 安装
```shell
make && make install
```


### 查询安装目录
```shell
whereis nginx
```


### 启动`nginx`
```shell
cd /usr/local/nginx/sbin
./nginx
```

### 重启`nginx`
```shell
./nginx -s reload
```

### 停止`nginx`
```shell
ps -ef | grep nginx

-- 从容停止： 
kill -QUIT xxx

-- 快速停止： 
kill -TERM xxx

-- 强制停止：
kill -9 nginx
```
