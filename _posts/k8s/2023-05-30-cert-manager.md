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


```
helm install fooval-cert-manager oci://registry-1.docker.io/bitnamicharts/cert-manager -n cert-manager --set installCRDs=true --version 0.11.2
```

```
kubectl get challenges -n fooval-git -o wide 

```

```

---
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: letsencrypt-fooval-git
  namespace: fooval-git
spec:
  acme:
    email: s15880277594@gmail.com
    # server: https://acme-staging-v02.api.letsencrypt.org/directory
    server: https://acme-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      # Secret resource that will be used to store the account's private key.
      name: fooval-git-account-key
    # Add a single challenge solver, HTTP01 using nginx
    solvers:
    - http01:
        ingress:
          name: fooval-drone
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: https-fooval-drone
  namespace: fooval-git
spec:
  dnsNames:
  - drone.fooval.cn # 要签发证书的域名
  issuerRef:
    kind: Issuer
    name: letsencrypt-fooval-git # 引用 Issuer，指示采用 http01 方式进行校验
  secretName: fooval-drone-tls # 最终签发出来的证书会保存在这个 Secret 里面
  
```