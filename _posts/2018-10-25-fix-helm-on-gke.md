---
layout: post
title:  "Fix Helm issues on GKE"
date:   2018-10-25 22:11:00 +0530
tags: [kubernetes, helm]
categories: [kubernetes, helm]
---
When installing Helm into GKE 1.10.7 it is not working fine. Every 'helm install' command fails with error:
```
Error: Get http://localhost:8080/version: dial tcp [::1]:8080: connect: connection refused
```
In Tiller pod logs there are following messages:
```
[storage/driver] 2018/10/25 14:10:27 query: failed to query with labels: Get http://localhost:8080/api/v1/namespaces/kube-system/configmaps?labelSelector=NAME%3Dconfig%2COWNER%3DTILLER: dial tcp [::1]:8080: connect: connection refused
```
The solution is like this:
```
kubectl -n kube-system edit deployment/tiller-deploy
```
Change automountServiceAccountToken: false to true.
After that 'helm install' finishes with 
```
Error: release config failed: namespaces "bell" is forbidden: User "system:serviceaccount:kube-system:default" cannot get namespaces in the namespace "bell": Unknown user "system:serviceaccount:kube-system:default"
```
To fix that do this:
```
kubectl -n kube-system create clusterrolebinding add-on-cluster-admin --clusterrole=cluster-admin --serviceaccount=kube-system:default
```
After that Helm works nicely.
