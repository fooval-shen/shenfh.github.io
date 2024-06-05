---
layout: post
title: Install Docker by Colima
author: shefh
catalog:  true
tags:
  - docker
  - k8s
header-img: img/blog-0.jpg
---

## docker

```
brew install colima

## coliam
https://github.com/abiosoft/colima

## start docker
colima start

## create VM with 2CPU, 2GiB memory and 10GiB storage.
colima start --cpu 2 --memory 2 --disk 10 -p docker

## modify an existing VM to 4CPUs and 4GiB memory.
colima stop
colima start --cpu 4 --memory 8


## Colima 的 ssh 命令进入虚拟机：
colima ssh
colima ssh -p docker


## 可以创建一个 k3s 作为 Kubernetes 运行时：

colima start --with-kubernetes --cpu 4 --memory 6 --disk 10 --mount ~/project:w

```



## k8s 

### Multiple k8s clusters

```

//Config file merge
KUBECONFIG=config1:config2 kubectl config view --flatten > $HOME/.kube/config

// View all clusters
kubectl config get-contexts 

// Switch to other cluster   
kubectl config use-context other-cluster-name} 

// look for more help
kubectl config --help  
```

## Configurations

### Set mirrors

```
## ~/.colima/xxx/colima.yaml
## mirror list: https://gist.github.com/y0ngb1n/7e8f16af3242c7815e7ca2f0833d3ea6
docker:
  registry-mirrors:
    - https://docker.m.daocloud.io
    - https://mirror.baidubce.com
    - https://docker.mirrors.ustc.edu.cn
    - https://mirror.gcr.io
```


### Assign reachable IP address to the virtual machine

```
## ~/.colima/xxx/colima.yaml
network:
  # Assign reachable IP address to the virtual machine.
  # NOTE: this is currently macOS only and ignored on Linux.
  # Default: false
  address: true
  
  # Custom DNS resolvers for the virtual machine.
  #
  # EXAMPLE
  # dns: [8.8.8.8, 1.1.1.1]
  #
  # Default: []
  dns:
    - 10.32.51.10
    - 10.32.51.12
    - 8.8.8.8
  
  # DNS hostnames to resolve to custom targets using the internal resolver.
  # This setting has no effect if a custom DNS resolver list is supplied above.
  # It does not configure the /etc/hosts files of any machine or container.
  # The value can be an IP address or another host.
  #
  # EXAMPLE
  # dnsHosts:
  #   example.com: 1.2.3.4
  dnsHosts: {}

```


If get the issure 'Error response from daemon: Get "https://registry-1.docker.io/v2/": net/http: TLS handshake timeout', configure the dns.