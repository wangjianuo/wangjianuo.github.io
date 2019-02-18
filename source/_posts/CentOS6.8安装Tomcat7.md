---
title: CentOS6.8安装Tomcat7
date: 2019-02-18 16:39:52
categories: "Linux"
tags: 
  - Linux
  - Tomcat
---



### 安装环境
```
- 系统环境：CentOS6.8 64bit
- Tomat版本：7.0.92
- IP: 192.168.9.128
```

### [下载](http://tomcat.apache.org/download-70.cgi)，上传，解压
```
[root@node01 ~]# tar -zxvf apache-tomcat-7.0.92.tar.gz -C /usr/local/tomcat/
```


### 启动
```
[root@node01 local]# cd tomcat/apache-tomcat-7.0.92/bin/
[root@node01 bin]# ./startup.sh 
Using CATALINA_BASE:   /usr/local/tomcat/apache-tomcat-7.0.92
Using CATALINA_HOME:   /usr/local/tomcat/apache-tomcat-7.0.92
Using CATALINA_TMPDIR: /usr/local/tomcat/apache-tomcat-7.0.92/temp
Using JRE_HOME:        /usr/local/java/jdk1.8.0_201
Using CLASSPATH:       /usr/local/tomcat/apache-tomcat-7.0.92/bin/bootstrap.jar:/usr/local/tomcat/apache-tomcat-7.0.92/bin/tomcat-juli.jar
Tomcat started.
```

### 停止
```
[root@node01 bin]# ./shutdown.sh 
Using CATALINA_BASE:   /usr/local/tomcat/apache-tomcat-7.0.92
Using CATALINA_HOME:   /usr/local/tomcat/apache-tomcat-7.0.92
Using CATALINA_TMPDIR: /usr/local/tomcat/apache-tomcat-7.0.92/temp
Using JRE_HOME:        /usr/local/java/jdk1.8.0_201
Using CLASSPATH:       /usr/local/tomcat/apache-tomcat-7.0.92/bin/bootstrap.jar:/usr/local/tomcat/apache-tomcat-7.0.92/bin/tomcat-juli.jar
[root@node01 bin]# 
```






### 设置防火墙
```
[root@node01 bin]# /sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
[root@node01 bin]# /etc/rc.d/init.d/iptables save
```

浏览器访问`192.168.9.128:8080`，打开`tomcat`页面，表示成功！

### 参考资料
```
http://tomcat.apache.org/download-70.cgi
```