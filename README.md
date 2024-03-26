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
## Deployment: Redis nodes

```
cd storage/redis/kubernetes/
kubectl apply -n redis -f ./redis/redis-configmap.yaml
kubectl apply -n redis -f ./redis/redis-statefulset.yaml

kubectl -n redis get pods
kubectl -n redis get pv

kubectl -n redis logs redis-0
kubectl -n redis logs redis-1
kubectl -n redis logs redis-2
```
## Test replication status

```
kubectl -n redis exec -it redis-0 -- sh
redis-cli 
auth a-very-complex-password-here
info replication
```
## Deployment: Redis Sentinel (3 instances)

```
kubectl apply -n redis -f ./sentinel/sentinel-statefulset.yaml

kubectl -n redis get pods
kubectl -n redis get pv
kubectl -n redis logs sentinel-0
```

## Testing failover

```
kubectl -n redis delete pods redis-0

#Check the sentinel logs
kubectl -n redis logs sentinel-0

#Check the ip of new master by getting the ips of the pods
kubectl -n redis get pods -o wide

#Verify by loggin in and checking info inside the redis-cli of the master
info replication
```
## Clean up

```
kind delete cluster --name redis
kubectl -n redis delete -f redis/redis-statefulset.yaml
kubectl -n redis delete -f sentinel/sentinel-statefulset.yaml
```
