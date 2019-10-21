---
title: 'import psycopg2报错undefined symbol: PQconninfo的解决方法'
date: 2019-10-18 16:30:13
tags:
- 笔记
---

## import psycopg2报错undefined symbol: PQconninfo的解决方法

在安装odoo后启动时，代码在执行import psycopg2导入psycopg2时报错，最后的描述信息是undefined symbol: PQconninfo，猜测应该是连接数据库时出错，网上搜索一番最后解决了这个问题。这个问题出现是psycopg2在运行过程中所用的libpq动态库和安装的postgresql的libpq动态库不一致导致的，所以要让psycopg2在运行过程中所用的libpq动态库指向安装的postgresql的libpq动态库才能解决这个问题。

### 第一步：查看psycopg2在运行过程中所用的libpq动态库

`$ ldd /path/to/packages/psycopg2/xxx.so | grep libpq`

上面的xxx不同的环境下名称可能不一样，具体以实际环境为准

### 第二步：进入动态库的目录，删除这个libpq动态库文件

`$ cd /yourpath/lib64`
`$ rm libpq.so.5`

### 第三步：给postgresql安装目录下lib目录下的libpq动态库在当前目录创建一个软链接：

`$ ln -s /your/postgresql/path/lib/libpq.so.5 ./`

### 第四步：重新安装psycopg2，问题解决。