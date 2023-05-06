---
layout: post
title: mysql 常用配置
author:  shenfh
catalog: true
date:  2020-03-10 07:17:17
tags:
    - sql
header-img: img/blog-0.jpg
---



## 查看慢查询是否打开

`show variables like 'slow%'; `

```
mysql> show variables like 'slow%'; 
+---------------------+-----------------------------------+
| Variable_name       | Value                             |
+---------------------+-----------------------------------+
| slow_launch_time    | 2                                 |
| slow_query_log      | ON                                |
| slow_query_log_file | /var/lib/mysql/data_back/slow.log |
+---------------------+-----------------------------------+
3 rows in set (0.10 sec)

```

### 打开慢查询配置

```
# mysql.conf

[mysqld]
slow_query_log = 1
slow_query_log_file = /var/lib/mysql/data_back/slow.log
long_query_time = 1
```

### 慢查询日志分销

```
# 显示前3条慢查询。
mysqldumpslow -t 3 /var/lib/mysql/data_back/slow.log | more \G;
```


## innodb_read_io_threads && innodb_write_io_threads

这两个参数决定了Innodb读写的I/O进程数，默认为4。
决定这两个参数数值的因素也有两个：cpu核数、应用场景中读写事务比例。

## innodb_purge_threads

## max_delayed_threads

## innodb_buffer_pool_size

innodb_buffer_pool_size 是非常重要的一个参数，用户配置Innodb 的缓冲池大小。如果数据库中只有Innodb表，则推荐配置量为总内存的75%。

## open_files_limit
该参数用于控制MySQL实例能够同时打开使用的文件句柄数目。建议值给成`102400`.

## max_connections

MySQL的最大连接数，如果服务器的并发连接请求量比较大，建议调高此值，以增加并行连接数量。 

## back_log

MySQL每处理一个连接请求时都会创建一个新线程与之对应。在主线程创建新线程期间，如果前端应用有大量的短连接请求到达数据库，MySQL会限制这些新的连接进入请求队列，由参数back_log控制。如果等待的连接数量超过back_log的值，则将不会接受新的连接请求，所以如果需要MySQL能够处理大量的短连接，需要提高此参数的大小。

建议值：`3000`

## query_cache_size

该参数用于控制MySQL query cache的内存大小。如果MySQL开启query cache，在执行每一个query的时候会先锁住query cache，然后判断是否存在于query cache中，如果存在则直接返回结果，如果不存在，则再进行引擎查询等操作。同时，insert、update和delete这样的操作都会将query cahce失效掉，这种失效还包括结构或者索引的任何变化。但是cache失效的维护代价较高，会给MySQL带来较大的压力。所以，当数据库不会频繁更新时，query cache是很有用的，但如果写入操作非常频繁并集中在某几张表上，那么query cache lock的锁机制就会造成很频繁的锁冲突，对于这一张表的写和读会互相等待query cache lock解锁，从而导致select的查询效率下降。

`建议关闭`。

## innodb_thread_concurrency 
 默认设置为 0,表示不限制并发数，这里推荐设置为0，更好去发挥CPU多核处理能力，提高并发量  
 
## wait_timeout

 服务器关闭非交互连接之前等待活动的秒数。在线程启动时，根据全局wait_timeout值或全局interactive_timeout值初始化会话wait_timeout值，取决于客户端类型(由mysql_real_connect()的连接选项CLIENT_INTERACTIVE定义)。参数默认值：28800秒（8小时。
 
 
 
## 参考文档
 
 https://blog.51cto.com/13581826/2122395
 

