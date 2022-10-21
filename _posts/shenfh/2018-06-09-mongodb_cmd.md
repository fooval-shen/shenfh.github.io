---
layout: post
title: MongoDB 常用命令
author:  shefh
catalog: true
date:  2018-06-09 00:00:00
tags:
    - mongoDB 
header-img: img/blog-0.jpg
---


## 超级用户相关
```
## 进入数据库admin
use admin

## 查看用户
db.system.users.find()

db.system.users.find({"user":"root"});

## 用户认证
db.auth("username","password")

## 退出登录
db.logout()

## 创建用户
db.createUser({user: 'username', pwd: 'xxxx', roles: [{role: 'readWrite', db: 'dbname'}]}); 

## 数据库管理员角色
dbAdmin -- 授予执行管理任务的特权
userAdmin -- 允许您在当前数据库上创建和修改用户和角色
dbOwner -- 拥有readWrite,dbAdmin,userAdmin权限.
readWrite -- 可以读写
read -- 只读权限

readAnyDatabase -- 与"read"角色相同，但适用于所有数据库
readWriteAnyDatabase -- 与"readWrite"角色相同，但适用于所有数据库
userAdminAnyDatabase -- 与"userAdmin"角色相同，但适用于所有数据库
dbAdminAnyDatabase -- 与"dbAdmin"角色相同，但适用于所有数据库

## 查看show用户
show users

## 查看所有数据库
show dbs

## 查看所有的collection
show collections

## 查看各collection的状态
db.printCollectionStats()

## 查看主从复制状态
db.printReplicationInfo()

## 查看profiling
show profile

## 拷贝数据库
db.copyDatabase("fromDB","toDB")

## 删除collection
db.collection_name.drop()

## 删除当前的数据库
db.dropDatabase()

```

### 登录命令

```
## 切换数据库
 use xxx;
 
## 登录mongodb

db.auth("username","password");

## 查看帮助
db.help()

```

### Collections 操作命令

```
## 创建collection
db.createCollection(name, options)

## 添加新的文档
db.collection_name.insert({"name":"name_value","id": "id_value"})

## 查找文档
db.collection_name.find({"name":"name_value","id": "id_value"})

## 更新文档
db.collection_name.update({"name":"name_value"},{"$set":{"id": "id_value2"}},upsert=true,multi=true)

## 删除文档
db.collection_name.remove({"name":"name_value"})

## 删除所有文档
db.collection_name.remove()
```