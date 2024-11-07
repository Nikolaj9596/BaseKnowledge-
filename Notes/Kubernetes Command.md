---
id: Kubernetes Command
aliases: []
tags:
  - kubernetes
  - command
---
## Command for delete error pods
```bash
kubectl get pods | grep Evicted | awk '{print $1}' | xargs kubectl delete pod
```
```bash
kubectl --namespace=production get pods -a | grep Evicted | awk '{print $1}' | xargs kubectl --namespace=production delete pod -o name

```
```bash
kubectl --namespace=food-box-main-dev-ns get pods -a | grep Evicted | awk '{print $1}' | xargs kubectl --namespace=food-box-main-dev-ns delete pod -o name

```
## Get auth token
```bash
  TOKEN=$(kubectl get secret -n "velo-ns" \ 
    $(kubectl get sa -n  "velo-ns" deploy \
      -o jsonpath='{.secrets[].name}') \
      -o jsonpath='{.data.token}')
```
