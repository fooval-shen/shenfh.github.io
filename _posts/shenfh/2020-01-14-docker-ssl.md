---
layout: post
title: Ubuntu Docker 配置 TLS 认证
author:  shenfh
catalog: true
date:  2020-01-14 07:17:17
tags:
    - docker 
    - 服务端
header-img: img/blog-0.jpg
---


默认情况下，Docker 通过监听本地的 unix socket 运行，同时还可以通过 TCP 进行通信，方便对 Docker 集群 管理。Docker 官方提供了通过 TLS 加密，来保证只有信任的客户端才能远程访问 Docker 服务。

采用私有 CA 签名证书。客户端只能够连接到该 CA 签名的证书和服务器。

## 使用 OpenSSL 创建 CA 、server 和 client


### 生成 CA 公钥和私钥

```
// 1. 生成 ca key  需要设置一个密码
openssl genrsa -aes256 -out ca-key.pem 4096

// 2. 创建 ca 公钥
openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem
```

### 创建 Server 端证书

```
//创建 server key
# openssl genrsa -out server-key.pem 4096

//证书签名请求文件 /CN后的填使用正式的ip或者域名
# openssl req -subj "/CN=120.x.x.x" -sha256 -new -key server-key.pem -out server.csr

//使用 CA 证书签名文件

// extfile.cnf IP:XXXX 允许访问的ip
# echo subjectAltName = DNS:test.com,IP:120.x.x.x,IP:120.x.x.1 >> extfile.cnf
# echo extendedKeyUsage = serverAuth >> extfile.cnf

//生成签名证书
# openssl x509 -req -days 365 -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem  -CAcreateserial -out server-cert.pem -extfile extfile.cnf
```


### 创建 client 证书

```
// 客户端证书
# openssl genrsa -out key.pem 4096
// 客户端签名请求文件
# openssl req -subj '/CN=client' -new -key key.pem -out client.csr
# echo extendedKeyUsage = clientAuth > extfile-client.cnf
// CA 签名
# openssl x509 -req -days 365 -sha256 -in client.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out cert.pem -extfile extfile-client.cnf
```

### server 端配置

* 上传服务器端证书`ca.pem` `server-cert.pem` `server-key.pem` 到`/root/docker/ssl`目录
* 修改docker启动配置

```
## 修改/lib/systemd/system/docker.service 文件

## ExecStart 后面添加
-H tcp://0.0.0.0:1275 -H unix:///var/run/docker.sock --tls  --tlscacert /root/docker/ssl/ca.pem --tlscert /root/docker/ssl/server-cert.pem  --tlskey /root/docker/ssl/server-key.pem

## 结果为
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:1275 -H unix:///var/run/docker.sock --tls  --tlscacert /root/docker/ssl/ca.pem --tlscert /root/docker/ssl/server-cert.pem  --tlskey /root/docker/ssl/server-key.pem

## 重启docker
systemctl daemon-reload
service docker restart
```

### 测试TLS连接

用客户端证书连接 `cert.pem`  `key.pem `

```
curl -k https://120.x.x.x:1275/info --cert cert.pem --key key.pem 
```

测试结果为

```
{"ID":"MZ4Z:N7JL:CQQS:PIX2:BMYN:SCUH:U7QH:SK3P:BMIK:VOXS:5BJE:BFV2","Containers":48,"ContainersRunning":47,"ContainersPaused":0,"ContainersStopped":1,"Images":66,"Driver":"overlay2","DriverStatus":[["Backing Filesystem","extfs"],["Supports d_type","true"],["Native Overlay Diff","true"]],...
```


### 参考地址
https://docs.docker.com/engine/security/https


