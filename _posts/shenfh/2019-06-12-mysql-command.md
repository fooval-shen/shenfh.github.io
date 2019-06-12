---
layout: post
title: mysql 常用命令
author:  shefh
catalog: true
date:  2019-06-11 07:17:17
tags:
    - mysql 
    - linux
header-img: img/blog-0.jpg
---


## 查看用户授权信息

```sql
SHOW GRANTS FOR 'test'@'%';
```

## 给用户授权增删改查某个库

```sql
GRANT SELECT, INSERT, UPDATE, DELETE ON `database`.* TO 'test'@'%'
```

## 给用户授权某个库所有权限

```sql
GRANT ALL PRIVILEGES ON `database`.* TO 'test'@'%' 
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

## 恢复数据库

```sql
登录数据库后选中某张表后执行下面命令
source /xx/xx/database.sql
```


