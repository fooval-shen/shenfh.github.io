---
layout: post
title: Helm Command
author:  fooval
catalog: true
date:  2022-07-12 00:00:00
tags:
    - k8s
header-img: img/home-building.jpg
---


## Command

### Add helm repo

```
helm repo add bitnami https://charts.bitnami.com/bitnami
```

### Install from repo

```
helm install my-release bitnami/mongodb -n default
```

### Install from local path

```
helm install my-release ./ -n default
```

### Install by dry-run

```
helm install my-release ./ -n default --dry-run
```

### Install with custom values

```
helm install ace-mongodb bitnami/mongodb --version 11.1.5 -n default --set persistence.existingClaim=mongodb-pvc --set auth.username=test --set auth.password=test --set auth.database=test --dry-run
```


### List all releases

```
helm list -n default
```

### Uninstall release

```
helm uninstall my-release -n default

```

### Package

```
helm package -d ./__chart__/ --version 1.0.0 --app-version 1.0.0 ./chart
```

### Helm Login

```
helm registry login  -u "username" -p "password"
```

### Helm publish

```
helm push ./__chart__/*.tgz oci://xxx.harbor-xxx.com/test
``