---
title: Linux丨CentOS6.8安装MySQL5-6
date: 2019-02-18 16:37:37
categories: "Linux"
tags: 
  - Linux
  - MySQL
---



### 安装环境
```
- 系统环境：CentOS6.8 64bit
- MySQL版本：5.6.42
- 安装方式：rpm
```

### 卸载自带的MySQL
```
[root@node01 ~]# rpm -qa| grep mysql
mysql-libs-5.1.73-7.el6.x86_64
[root@node01 ~]# rpm -e --nodeps mysql-libs-5.1.73-7.el6.x86_64
[root@node01 ~]# rpm -qa | grep mysql
[root@node01 ~]# 
```

### 创建MySQL安装路径
```
[root@node01 ~]# mkdir /usr/local/mysql
```

### MySQL[下载](https://dev.mysql.com/downloads/mysql/5.6.html#downloads) 
```
MySQL Community Server 5.6.43
Red Hat Enterprise Linux 6 / Oracle Linux 6 (x86, 64-bit)
```

### 上传RPM
只需要把其中的3个`PRM`上传到`/usr/local/mysql/`下就行了，三者分别是：
- `mysql-server`：数据库服务器
- `mysql-client`：客户端
- `mysql-devel`：开发用到的库以及包含文件

```
-rw-r--r--. 1 root root  57713020 2月  15 10:18 MySQL-server-5.6.43-1.el6.x86_64.rpm
-rw-r--r--. 1 root root  19026428 2月  15 10:18 MySQL-client-5.6.43-1.el6.x86_64.rpm
-rw-r--r--. 1 root root   3459304 2月  15 10:18 MySQL-devel-5.6.43-1.el6.x86_64.rpm
```

### 安装MySQL
```
[root@node01 mysql]# rpm -ivh MySQL-server-5.6.43-1.el6.x86_64.rpm --force --nodeps
[root@node01 mysql]# rpm -ivh MySQL-client-5.6.43-1.el6.x86_64.rpm --force --nodeps
[root@node01 mysql]# rpm -ivh MySQL-devel-5.6.43-1.el6.x86_64.rpm --force --nodeps
```

安装成功，可以看到一些信息：
```
A RANDOM PASSWORD HAS BEEN SET FOR THE MySQL root USER !
You will find that password in '/root/.mysql_secret'.

You must change that password on your first connect,
no other statement but 'SET PASSWORD' will be accepted.
See the manual for the semantics of the 'password expired' flag.

Also, the account for the anonymous user has been removed.

In addition, you can run:

  /usr/bin/mysql_secure_installation

which will also give you the option of removing the test database.
This is strongly recommended for production servers.

See the manual for more instructions.

Please report any problems at http://bugs.mysql.com/

The latest information about MySQL is available on the web at

  http://www.mysql.com

Support MySQL by buying support/licenses at http://shop.mysql.com

New default config file was created as /usr/my.cnf and
will be used by default by the server when you start it.
You may edit this file to change server settings

WARNING: Default config file /etc/my.cnf exists on the system
This file will be read by default by the MySQL server
If you do not want to use this, either remove it, or use the
--defaults-file argument to mysqld_safe when starting the server
```

大概说了2个事：

- `mysql`默认配置文件是`/usr/my.cnf`。
- `root`用户给分配一个随机密码，到`/root/.mysql_secret`里面看。

```
[root@node01 mysql]# cat /root/.mysql_secret
# The random password set for the root user at Fri Feb 15 10:24:18 2019 (local time): PUMBwY3uVAbm0NNV
```


### 查看状态，启动
```
[root@node01 mysql]# service mysql status
MySQL is not running                                       [失败]
--
[root@node01 mysql]# service mysql restart
MySQL server PID file could not be found!                  [失败]
Starting MySQL.                                            [确定]
```

### 登录，修改密码
```
[root@node01 mysql]# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 1
Server version: 5.6.43

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
mysql>
mysql> SET PASSWORD=PASSWORD('123456');
Query OK, 0 rows affected (0.01 sec)

mysql> exit
Bye

```
用`root`和`123456`重新登录，如果可以登录，表示成功了！




### 远程访问
- 修改表

```
mysql> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
mysql> update user set host='%' where user = 'root';
ERROR 1062 (23000): Duplicate entry '%-root' for key 'PRIMARY'
mysql> 
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

mysql> select host, user from user;
+-----------+------+
| host      | user |
+-----------+------+
| %         | root |
| 127.0.0.1 | root |
| ::1       | root |
| node01    | root |
+-----------+------+
4 rows in set (0.00 sec)

mysql> exit;
```

- 防火墙

```
[root@node01 mysql]# iptables -I INPUT -s 0/0 -p tcp --dport 3306 -j ACCEPT
[root@node01 mysql]# iptables -L -n|grep 3306
ACCEPT     tcp  --  0.0.0.0/0            0.0.0.0/0           tcp dpt:3306
```

测试远程连接，成功！


### 开机自启
```
-- 加入到系统服务
[root@node01 mysql]# chkconfig --add mysql
-- 自动启动
[root@node01 mysql]# chkconfig mysql on
```

### 参考资料
```
https://segmentfault.com/a/1190000011454296
https://www.cnblogs.com/weifeng1463/p/7941625.html
https://kfcman.iteye.com/blog/2432058
https://blog.csdn.net/dong_18383219470/article/details/57418629
-- 
https://dev.mysql.com/downloads/mysql/
http://mirrors.sohu.com/mysql/MySQL-5.6/
```
