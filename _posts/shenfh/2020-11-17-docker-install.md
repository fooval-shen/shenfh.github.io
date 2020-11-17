---
layout: post
title:  ubuntu18 安装docker
author:  shenfh
catalog: true
date:  2020-11-17 07:17:17
tags:
    - docker
header-img: img/blog-0.jpg
---



## Docker 安装

#### 卸载旧版本的Docker

```
sudo apt-get remove docker \
               docker-engine \
               docker.io
```

#### 安装HTTPS CA 证书

```
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```
 
#### 使用国内源
 
 鉴于国内网络问题，强烈建议使用国内源，官方源请在注释中查看。

为了确认所下载软件包的合法性，需要添加软件源的 GPG 密钥。

```
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -


// 官方源
//  
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```


#### 向 source.list 中添加 Docker 软件源

```
sudo add-apt-repository \
    "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu \
    $(lsb_release -cs) \
    stable"


// 官方源
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"

```

#### 安装 Docker CE

```
sudo apt-get update

sudo apt-get install docker-ce
```


#### 启动 Docker CE

```
sudo systemctl enable docker
sudo systemctl start docker
```


#### 将当前用户加入 docker 组

```
sudo usermod -aG docker $USER
```


## docker-compose 安装

#### 安装脚本

```
sudo curl -L https://github.com/docker/compose/releases/download/1.24.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

