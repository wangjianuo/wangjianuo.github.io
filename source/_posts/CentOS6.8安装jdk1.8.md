---
title: CentOS6.8安装jdk1.8
date: 2019-02-18 16:32:51
categories: "Linux"
tags: 
  - Linux
  - JDK
---


### 安装环境
```
- 系统环境：CentOS6.8 64bit
- JDK版本：1.8.0_201
```

### 卸载OpenJDK
```
-- 查看
[root@node01 ~]# rpm -qa | grep java
tzdata-java-2016c-1.el6.noarch
java-1.7.0-openjdk-1.7.0.99-2.6.5.1.el6.x86_64
java-1.6.0-openjdk-1.6.0.38-1.13.10.4.el6.x86_64
-- 卸载
[root@node01 ~]# rpm -e --nodeps java-1.7.0-openjdk-1.7.0.99-2.6.5.1.el6.x86_64
[root@node01 ~]# rpm -e --nodeps java-1.6.0-openjdk-1.6.0.38-1.13.10.4.el6.x86_64
-- 检验
[root@node01 ~]# rpm -qa | grep java
tzdata-java-2016c-1.el6.noarch
```

### 创建JDK文件夹，上传，解压
```
[root@node01 ~]# mkdir /usr/local/java
[root@node01 ~]# tar -zxvf jdk-8u201-linux-x64.tar.gz -C /usr/local/java/
```

### 配置环境变量
```
[root@node01 ~]# vim /etc/profile
--
# SET JDK
JAVA_HOME=/usr/local/java/jdk1.8.0_201
CLASSPATH=.:$JAVA_HOME/lib.tools.jar
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH
```

### 重新加载配置文件
```
[root@node01 ~]# source /etc/profile
```

### 验证是否成功
```
[root@node01 ~]# java -version
java version "1.8.0_201"
Java(TM) SE Runtime Environment (build 1.8.0_201-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.201-b09, mixed mode)
---
java -version
java 
javac
```




### 参考资料
```
https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

```