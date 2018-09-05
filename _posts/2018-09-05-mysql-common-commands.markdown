---
layout: post
title:  "MySQL常用命令"
date:   2018-09-05 22:30:00 +0800
categories: MySQL
---

# 连接与断开
```shell
# 启动数据库
$ mysqld

# 连接数据库
$ mysql -h <主机> -u <用户名> -p

# 断开数据库
mysql> exit
```

# 数据库
```shell
# 列出所有数据库
mysql> show databases;

# 创建数据库
mysql> create database <数据库名>;

# 删除数据库
mysql> drop database <数据库名>;

# 使用数据库
mysql> use <数据库名>;
```

# 表
```shell
# 创建表
mysql> create table <表名> (
	id int(10) not null primary key auto_increment,
	字段2 类型,
	字段3 类型
	);

# 删除表
mysql> drop table <表名>;
```

# JOIN
```shell
# 内连接
# 两表同时符合条件的交集，读取两表中符合条件的记录并拼接为一个结果集。
mysql> select * from <表名一> inner join <表名二> on <条件>;

# 左连接
# 左表全集，右表符合条件的子集。读取左表(表名一)中所有记录，并将右表中满足条件的记录拼接为一个结果集，不匹配的字段补为NULL。
mysql> select * from <表名一> left join <表名二> on <条件>;

# 右连接
# 右表全集，左表符合条件的子集。读取右表(表名二)中所有记录，并将左表中满足条件的记录拼接为一个结果集，不匹配的字段补为NULL。
mysql> select * from <表名一> right join <表名二> on <条件>;
```