# kubernetes-redis

Create a cluster with [kind](https://kind.sigs.k8s.io/docs/user/quick-start/)
```
kind create cluster --name redis --image kindest/node:v1.29.2 #the node version is of K8
```
## Namespace
```
kubectl create ns redis
```
## Storage Class
```
kubectl get storageclass
```