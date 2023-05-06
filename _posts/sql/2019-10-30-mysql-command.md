---
layout: post
title: mysql 常用命令
author: shefh
catalog:  true
header-img: img/home-mountain.jpg
tags:
    - sql
---

## 创建数据库

#### 命令

```
create database `xxx` default character set utf8mb4 collate utf8mb4_unicode_ci;

```

## 创建用户

#### 命令

```
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```

#### 说明
* `username`：将要创建的用户名
* `host`：指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以从任意远程主机登陆，可以使用通配符%
* `password`：登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器

## 用户授权

#### 命令

```
grant all privileges on *.* to 'username'@'host' with grant option;
```

#### 说明

* `privileges`：用户的操作权限，如SELECT，INSERT，UPDATE等，如果要授予所的权限则使用ALL
* `databasexx`：数据库名
* `tablexx`：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用`*`表示，如`*.*`


## 设置与更改用户密码

#### 命令
```
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
```

* 修改当前登陆的用户密码

```
SET PASSWORD = PASSWORD("xxxxxxxx");
```

* 例子

```
SET PASSWORD FOR 'xxx'@'%' = PASSWORD("xxxxxxxx");
```


## 备份数据库

### 备份整个数据库

```
mysqldump -u 用户名 -p 数据库名 > 导出的文件名

eg: mysqldump -u test -p database > database.sql;
```

### 备份某一张表

```sql
    mysqldump -u 用户名 -p 数据库名 表名> 导出的文件名
```

### 查询结果保存到`xls`


* 查看mysql outfile 保存的文件夹路劲

```
SHOW VARIABLES LIKE "secure_file_priv";
```

* 查询结果保存到xls文件

```
select * 
into outfile '/var/lib/mysql-files/lt.xls'
from xxx;
```
* 转码

```
iconv -futf8 -tgb2312 -o lt.xls lt-1.xls
```

## 恢复数据库

```sql
登录数据库后选中某张表后执行下面命令
source /xx/xx/database.sql
```

## 查看表大小

### 查看数据库容量大小

```sql
select
table_schema as '数据库',
table_name as '表名',
table_rows as '记录数',
truncate(data_length/1024/1024, 2) as '数据容量(MB)',
truncate(index_length/1024/1024, 2) as '索引容量(MB)'
from information_schema.tables
order by data_length desc, index_length desc;
```

### 查看指定库的容量大小

```sql
select
table_schema as '数据库',
table_name as '表名',
table_rows as '记录数',
truncate(data_length/1024/1024, 2) as '数据容量(MB)',
truncate(index_length/1024/1024, 2) as '索引容量(MB)'
from information_schema.tables
where table_schema='mysql'
order by data_length desc, index_length desc;
```

## 故障排除

### 查看所有mysql进程

```
show full processlist; 
```


### 查询是否锁表

```
show OPEN TABLES where In_use > 0;

# 查看正在锁的事务
SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS; 
```


### 查看等待锁的事务

```
SELECT * FROM INFORMATION_SCHEMA.INNODB_TRX;

SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCK_WAITS; 
```

### 查看服务器状态

```
show status like '%lock%';
```
### 查看超时时间

```
show variables like '%timeout%';
```

### 碎片整理

```
// For innoDB
ALTER TABLE xxxx ENGINE= INNODB;


// For MyISAM
OPTIMIZE TABLE xxx;
```



### Connect 

#### Connect to host

```
## Connect to remote host
mysql -u user -h host -p -P port

## Connect to localhost
mysql -u user -p
```

#### Quit

```
quit;

OR

exit;
```

#### show mysql version and current time

```
select version(), now();

## eg:
+-----------+---------------------+
| version() | now()               |
+-----------+---------------------+
| 8.0.27    | 2022-10-29 14:44:59 |
+-----------+---------------------+
1 row in set (0.00 sec)
```

#### cancle execute 

If your decide you do not want to execute a query that you are in the process of entering, cancle it by typing `\c`:

```
mysql> select
    -> version()
    -> now()
    -> \c;
ERROR: 
No query specified
```

### Command

#### Create a Database

```
create database `xxx` default character set utf8mb4 collate utf8mb4_unicode_ci;
```

#### Show table fields

```
mysql> describe user;
+------------------------------------+---------------+------+-----+---------+-------+
| Field                              | Type          | Null | Key | Default | Extra |
+------------------------------------+---------------+------+-----+---------+-------+
| user_id                            | varchar(355)  | NO   | PRI | NULL    |       |
| token                              | varchar(1024) | YES  |     | NULL    |       |
+------------------------------------+---------------+------+-----+---------+-------+
```

Show table create DDL

```
show create table user;
```

#### Date Calculation Functions

```
## Calculate age
timestampdiff(year, date, curdate())
 
# eg:
mysql> select create_time, curdate() as cur_date,timestampdiff(day,create_time,curdate()) as day_dif from out_order limit 3;
+---------------------+------------+---------+
| create_time         | cur_date   | day_dif |
+---------------------+------------+---------+
| 2018-08-24 10:29:19 | 2022-10-30 |    1527 |
| 2018-08-24 10:29:19 | 2022-10-30 |    1527 |
| 2018-08-24 10:29:19 | 2022-10-30 |    1527 |
+---------------------+------------+---------+
3 rows in set (0.00 sec)
 
```