---
layout: post
title: k8s YAML note
author:  fooval
catalog: true
date:  2022-11-01 00:00:00
tags:
    - k8s
header-img: img/home-building.jpg
---


## PersistentVolumes

### Local Storage PersistentVolume

```yaml
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: ace-es-master0-pv
  labels:
    volume: ace-es-master-pv
spec:
  capacity:
    storage: 2Gi
  hostPath:
    path: /data/disks/es/master0
    type: DirectoryOrCreate
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain
  storageClassName: local-storage
  volumeMode: Filesystem
  nodeAffinity:
    required:
      nodeSelectorTerms:
        - matchExpressions:
            - key: kubernetes.io/hostname
              operator: In
              values:
                - node01
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: ace-es-master0-pvc
  labels:
    app: ace-es-master-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 2Gi
  volumeName: ace-es-master0-pv
  storageClassName: local-storage
  volumeMode: Filesystem

```

### User PersistentVolume With `Selector`

```yaml

apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: ace-elasticsearch-master  
spec:
  replicas: 2
  template:
    spec:
      containers:
        - name: elasticsearch
          image: docker.io/bitnami/elasticsearch:8.4.3
          volumeMounts:
            - name: data
              mountPath: /bitnami/elasticsearch/data
  volumeClaimTemplates:
    - kind: PersistentVolumeClaim
      apiVersion: v1
      metadata:
        name: data
      spec:
        accessModes:
          - ReadWriteOnce
        selector:
          matchLabels:
            volume: ace-es-master-pv
        resources:
          requests:
            storage: 2Gi
        storageClassName: local-storage
        volumeMode: Filesystem
```

### `NFS` PersistentVolume 

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: maven-pv
spec:
  capacity:
    storage: 2Gi
  nfs:
    server: 172.16.0.16
    path: /data/maven
    type: DirectoryOrCreate
  accessModes:
    - ReadWriteOnce
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  mountOptions:
    - vers=4,port=2049
  volumeMode: Filesystem
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: maven-pvc
spec:
  accessModes:
    - ReadWriteOnce
    - ReadWriteMany
  resources:
    requests:
      storage: 2Gi
  volumeName: maven-pv
  storageClassName: ''
  volumeMode: Filesystem
```