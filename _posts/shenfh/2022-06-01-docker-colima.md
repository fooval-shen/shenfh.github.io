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
docker:
  registry-mirrors:
    - https://registry.hub.docker.com
    - https://mirror.baidubce.com
    - https://docker.mirrors.ustc.edu.cn
    - https://mirror.gcr.io
```