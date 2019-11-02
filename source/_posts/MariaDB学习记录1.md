---
title: MariaDB安装及一些准备工作
date: 2019-10-31 23:18:10
tags:
- 数据库
---

MariaDB数据库是MySQL数据库作者在MySQL被oracle收购之后，基于MySQL社区版而开发的分支。一直有传言MySQL被收购之后有闭源的风险， MySQL的作者正是不满这种做法才决定开发MariaDB数据库，而MariaDB和MySQL基本上是一致的，其守护进程也叫mysqld，所以对于考虑商业风险的公司，从MySQL迁移到MariaDB可以做到无缝迁移。只有使用到MariaDB有而MySQL没有的特性时才需要特别需要学习相关的知识。{% link MySQL下载地址 http://dev.mysql.com/downloads/mysql/ external %} {% link MariaDB下载地址 https://downloads.mariadb.org/mariadb/ external %}

### 确定MariaDB有没有安装

1. 在 Linux 或 Mac 系统

`ps aux | grep mysql`

2. windows系统

`tasklist /fi "IMAGENAME eq mysqld"`

上面只是确定mysqld有没有运行，可以用`mysqladmin -p version status`来进一步确定有没有安装

### 安装后给root设置初始密码

改变 root 密码的方法有多种，比如使用管理工具 mysqladmin：

`mysqladmin -u root -p flush-privileges password`

初始密码没有设置，直接回车，然后输入要设置的密码

还可以客户端连接更改密码：

查看所有用户：
`mysql -u root -p -e "SELECT User,Host FROM mysql.user;"`
更改密码及删除一些用户：

`mysql -u root -p -e "SET PASSWORD FOR 'root'@'127.0.0.1' PASSWORD('new_pwd');"`
`mysql -u root -p -e "SET PASSWORD FOR 'root'@'localhost' PASSWORD('new_pwd');"`
`mysql -u root -p -e "DROP USER 'root'@'%';"`

修改完成后用下面的命令让密码生效：
`mysqladmin -u root -p flush-privileges`

### 创建用户及赋予用户权限：

一般都是新建一些用户来操作数据库，而不是用root用户
```
mysql -u root -p -e "GRANT USAGE ON *.*
TO 'russell'@'localhost'
IDENTIFIED BY 'Rover#My_1st_Dog&Not_Yours!';"
```
赋予用户查询的权限：

`mysql -u root -p -e "GRANT SELECT ON *.* TO 'russell'@'localhost';"`

查询用户的权限：
`mysql -u root -p -e "SHOW GRANTS FOR 'russell'@'localhost' \G"`

赋予用户所有的权限：

`mysql -u root -p -e "GRANT ALL ON *.* TO 'russell'@'localhost';"`