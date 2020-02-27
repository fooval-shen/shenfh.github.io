---
layout: post
title: Ubuntu 常用配置
author:  shenfh
catalog: true
date:  2020-02-27 07:17:17
tags:
    - linux 
header-img: img/blog-0.jpg
---

## 修改镜像源

修改镜像源配置文件`/etc/apt/sources.list`,内如如下。修改之前可以先做一个备份。

```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```


执行如下命令使镜像源生效

```
sudo apt-get update
sudo apt-get upgrade
```

## 启用root用户

* 使用`sudo passwd root` 设置root用户密码
* `su root` 命令测试是否可以进入root用户

## 启用 root用户登陆ssh

* 修改`/etc/ssh/sshd_config` 文件,修改内如如下

```
# Authentication:
LoginGraceTime 10m
PermitRootLogin yes
StrictModes yes
```

* 执行`sudo service ssh restart` 重启ssh 生效

