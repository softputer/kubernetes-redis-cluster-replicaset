# Kubernetes Redis Cluster By ReplicaSet

This repo is inspired by Kelsey Hightower's [kubernetes-redis-cluster](https://github.com/kelseyhightower/kubernetes-redis-cluster).  But I have remove the usage of google cloud platform disk which makes it more flexible to deploy. And I have include redis.conf in the new image by the Dockerfile in the repo.

### Create Redis Nodes

```
kubectl create -f replicasets
```

### Create Redis Services

```
kubectl create -f services
```

### Connect Nodes

```
kubectl run -i --tty redis-trib --image=softputer/redis-trib \
  --restart=Never /bin/bash
```

```
./redis-trib.rb create --replicas 1 \
  10.131.242.1:6379 \
  10.131.242.2:6379 \
  10.131.242.3:6379 \
  10.131.242.4:6379 \
  10.131.242.5:6379 \
  10.131.242.6:6379
```

The IP above can be either pod ip or service ip. After executing the command successfully, you will get a reds cluster with 3 masters and 3 slaves.

### Add a new node

```
kubectl create -f replicaset/redis-7.yaml
```

```
kubectl create -f services/redis-7.yaml
```

```
CLUSTER MEET 10.131.242.7 6379
```
