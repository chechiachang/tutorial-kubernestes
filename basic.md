Basic
===

# Create a Cluster

### Key words

Kubernetes Cluster
Containerized Application
Master
Node
Kuberlet

### Kubernetes Cluster
1. A high available cluster of machines
2. Work as a unit
3. Master coordinates the cluster
4. Nodes run applications

### Containerized Application 
1. Packaged decoupling from individual hosts
2. More flexible and available

Kubernetes automates distribute and schedule application containers across cluster

### Master
1. responsible for managing the cluster
2. scheduling application, maintaining applications' desired state
3. rolling out new update
4. exposes K8s API

### Node
1. each node has a Kubelet, managing node and communicating with the master through K8s API
2. node has tools for container operation, such as docker, rkt...
3. a cluster should have 3 nodes at least for production

### Start a Cluster
1. tell master to start the application containers
2. master schedules the containers to run on nodes
3. End use use API to interact with cluster

```
minikube version
minikube start

kubectl cluster-info
kubectl get nodes
```

# Deploy an App

### Key Words

Deployment
Deployment Controller
Container Image
Number of replicas
Proxy
Pod

### Deployment
1. responsible for creating and updating instances of applications
2. master schedules the application instances, deployement creates onto nodes
3. Deployment Controller monitors instances

### Self-healing
1. If an instance is down or is deleted, the controller replaces it.
2. Pre-orchestration world, we use installation scripts without recovery from failure. Deployement provides creating and recovery

### Deploy an App

```
kubectl get nodes --help
kubectl run kubernetes-bootcamp --image=docker.io/jocatalin/kubernetes-bootcamp:v1 --port=8080

kubectl get deployments
kubectl proxy

kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}'
export POD_NAME=${kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}' )
curl http://localhost:8081/api/v1/proxy/namespaces/default/pods/$POD_NAME
```

# Pods and Nodes

### Key Words

Kubernetes Pod
Volumes

Kubernetes Node


### Pod = containers + resources
1. Atomic unit of Kubernetes platform
1. Kubenetes creates a pod to host application instance as user create a deployment
2. A Pod is a Kubernetes abstraction that represent a group of containers(Docker, rkt...)
3. with some shared resources
..1. shared storage, as Volumes
..2. Networking, as a unique cluster IP address
..3. Information about how to run each containers, such as container images verion or ports
4. A Pod is an logical host for application, in this host containers are tighly coupled
5. The containers in a Pod shared resources and are always co-located, co-scheduled, run in a shared context of the same Node
6. Pod is tied to a Node when created
7. Many identical Pods runs on different Nodes in case of Node failure

# Node

1. A worker machine managed by the Master
2. Master automatically handles scheduling Pods across the Nodes in cluster
3. A Pod runs on a Node
4. A node runs at least
..1. Kubelet, a process for communication between Master and the Nodes, managing Pods and Containers on a Node
..2. A container runtime(Docker, rkt...) responsible for pulling image, unpacking container, and running application

# Overview

User creates deployment in a cluster,
deployment creates Pod in a Node,
with containers in the Pod

1. Node1
2. Node2
3. Node3
..0. Kubelet node processes
..1. Pod1 10.10.10.1
..2. Pod2 10.10.10.2
..3. Pod3 10.10.10.3
....1. Volume1
....2. Containerized app1
..3. Volume2
..4. Containerized app2

```
kubectl get
kubectl describe
kubectl logs
kubectl exec
```
