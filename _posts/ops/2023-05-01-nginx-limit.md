---
layout: post
title: Nginx 限流
author: fooval
catalog:  true
header-img: img/home-mountain.jpg
tags:
    - linux
---


## limit_req_zone

* limit_req_zone 用来限制单位时间内的请求数，即速率限制,采用的漏桶算法 “leaky bucket”。
* limit_req_conn 用来限制同一时间连接数，即并发限制。

### 用法说明
```http
http {
    limit_req_zone $binary_remote_addr zone=one:10m rate=1r/s;
    server {
        location /search/ {
            limit_req zone=one burst=5 nodelay;
        }
}

```

#### limit_req_zone参数说明 
```
limit_req_zone $binary_remote_addr zone=one:10m rate=1r/s;
```
* 第一个参数：$binary_remote_addr 表示通过remote_addr这个标识来做限制，“binary_”的目的是缩写内存占用量，是限制同一客户端ip地址。
* 第二个参数：zone=one:10m表示生成一个大小为10M，名字为one的内存区域，用来存储访问的频次信息。
* 第三个参数：rate=1r/s表示允许相同标识的客户端的访问频次，这里限制的是每秒1次，还可以有比如30r/m的。
* 
#### limit_req参数说明

```
limit_req zone=one burst=5 nodelay;
```

* 第一个参数：zone=one 设置使用哪个配置区域来做限制，与上面limit_req_zone 里的name对应。
* 第二个参数：burst=5，burst爆发的意思，这个配置的意思是设置一个大小为5的缓冲区当有大量请求（爆发）过来时，超过了访问频次限制的请求可以先放到这个缓冲区内。
* 第三个参数：nodelay，如果设置，超过访问频次而且缓冲区也满了的时候就会直接返回503，如果没有设置，则所有请求会等待排队。


## limit_conn_zone

这个模块用来限制单个IP的请求数。并非所有的连接都被计数。只有在服务器处理了请求并且已经读取了整个请求头时，连接才被计数

```
http
{
    # 根据客户端ip进行限制，区域名称为perip，总容量为10m
    limit_conn_zone $binary_remote_addr zone=perip:10m;
    limit_conn_zone $server_name zone=perserver:10m;
    server
    {
        # 使用perip区域名称(zone name)，同一时间并发数不得超过10
        limit_conn perip 10;
        limit_conn perserver 100;
    }
}

```


#### limit_conn_zone参数说明

```
 limit_conn_zone $binary_remote_addr zone=perip:10m;
```

* 第一个参数：$binary_remote_addr 表示通过remote_addr这个标识来做限制.
* 第二个参数：zone=perip:10m表示生成一个大小为10M，名字为perip的内存区域，用来存储访问的频次信息。


## limit_rate

```
#声明限制请求传输速率在超过500KB后启用
limit_rate_after 500K;
#声明每个请求会被限速为100K的网速
limit_rate 100K;
```

