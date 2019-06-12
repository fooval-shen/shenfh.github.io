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

