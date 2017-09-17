Minikube
===

### Start Minikube

```
minikube start --vm-driver=xhyve
kubectl cluster-info dump
```

### Create Docker
```
mkdir hellonode && cd hellonode
vim server.js

vim Dockerfile
eval $(minikube docker-env)
docker build -t hello-node:v1
```

### Create a Deployment
```
kubectl run hello-node --image=hello-node:v1 --port=8080
kubectl get deployments
kubectl get pods
kubectl get events
kubectl config view
```

### Update app

```
vim server.js
docker build -t hello-node:v2 .
kubectl set image deployment/hello-node hello-node=hello-node:v2
minikube service hello-node(?)
```

### Clean up

```
kubectl delete service hello-node
kubectl delete deployment hello-node
minikube start
```
