---
layout: post
title: Flux CD Install
author: shefh
catalog:  true
tags:
  - GitOps
  - k8s
header-img: img/home-building.jpg
---

# Flux CD install guide


## Install with bootstrap

Using the flux bootstrap command you can install Flux on a Kubernetes cluster and configure it to manage itself from a Git repository.

If the Flux components are present on the cluster, the bootstrap command will perform an upgrade if needed. The bootstrap is idempotent, it’s safe to run the command as many times as you want.

```
flux bootstrap git --url='ssh://git@git.xxxx/test-gitops.git' --private-key-file=/xxx/.ssh/xxx_rsa --branch=test-pod --namespace=flux
```

### What does the bootstrap do?

 * Install all the components needed by the flux. You can run `kubectl get pods -n flux` to show all the pods that the flux installed.
 * Create git access key or save access key provided by `--private-key-file`. You can run `kubectl get secrets -n flux` command to show the access key.
 * Create GitRepository by git URL provided by `--url`. You can locate it in `Custom Resources` -> `source.toolkit.fluxcd.io` -> `GitRepository` by [Lens](https://github.com/lensapp/lens). It tells the flux which branch to watch.
 * Create flux `kustomization`. It is the most critical configuration. It tells the flux which file or path to watch.
 * Upload flux system configuration. The first time you run bootstrap,flux will upload all the flux system configurations to your git repository.

## Manual installation

Install with bootstrap is the best meth for an empty GitRespository. But sometimes, we provide a repository already deployed in another cluster, or we just need to add a new repository for the current cluster. Thes times, we need manual installation.


### Install flux
If the cluster is not install Flux, run the command to install:

```
flux install --namespace=flux
```

### Add new git repository

* Add Deployment to you git repository in branch flux-demo. 

```
# nginx.yaml

apiVersion: apps/v1 
kind: Deployment
metadata:
  name: nginx-fooval
  namespace: default
spec:
  selector:
    matchLabels:
      app: nginx-fooval
  replicas: 1 # tells deployment to run 1 pods matching the template
  template: # create pods using pod definition in this template
    metadata:
      labels:
        app: nginx-fooval
    spec:
      containers:
      - name: tnginx-fooval
        image: nginx
        ports:
        - containerPort: 80
```

* Create repository

```
flux create source git fooval-gitops --url=ssh://xxx/test-gitops.git --branch=flux-demo --interval=1m --private-key-file=/xxxxn/.ssh/xx_rsa --namespace=flux
```

### Add kustomize

```
apiVersion: kustomize.toolkit.fluxcd.io/v1beta2
kind: Kustomization
metadata:
  name: fooval-gitops
  namespace: flux
spec:
  force: false
  interval: 10m0s
  path: ./
  prune: true
  sourceRef:
    kind: GitRepository
    name: fooval-gitops
```

## Get all ks / Show errors

```
flux get ks -A / flux get kustomizations -A
```

## Show kustomizations all file

```
flux tree ks fooval-gitops
```

## Uninstall Flux

```
flux uninstall --namespace=flux-system

flux uninstall --namespace=flux --keep-namespace
```
