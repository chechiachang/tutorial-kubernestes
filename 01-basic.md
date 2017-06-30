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

### Node

1. A worker machine managed by the Master
2. Master automatically handles scheduling Pods across the Nodes in cluster
3. A Pod runs on a Node
4. A node runs at least
..1. Kubelet, a process for communication between Master and the Nodes, managing Pods and Containers on a Node
..2. A container runtime(Docker, rkt...) responsible for pulling image, unpacking container, and running application

### Overview

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
kubectl logs $POD_NAME
kubectl exec -it $POD_NAME bash
```

# Expose app Publicly

### Key words
Pod Lifecycle
Replication Controller
Kubernetes Service
Label Selector
Service Spec
Expose Type
Selector

### Pod Lifecycle
1. Node dies, Pods lost
2. Repilcation Contrller dynamically drive the cluster to disired state via creating new Pods
3. Front-End system should not care about backend replicas amoung Pods, so there should be a way to reconciling changes amoung Pods

### Service
1. A Service is a Kubernetes abstraction
..1. defines a logical set of Pods
..2. a policy by which to access them
2. Enable loose coupling between Pods
3. Defined using YAML or JSON
4. Label Selector determine a sets of Pods targeted by a Service
5. Pods has unique IP address. Service export IPs outside the cluster in different types
..1. ClusterIP: expose Service on an internal IP in the Cluster
..2. NodePort: expose Service on the same port of selected Node using NAT. Service avaliable from outside cluster using : .
..3. Load Balancer: Create an external load balancer in the current cloud and assign an external IP.
..4. External Name: Expose service using external name by returning CNAME record. No proxy used. 

### Expose

```
kubectl get pods
kubectl get services
kubectl expose deployment/kubernates-bootcamp --type="NodePort" --port 8080
kubectl get services
curl host:port

kubectl delete service [$SERVICE_NAME][-l label=label]
```

# Scale

Desired
Current
Up-To-Date
Available

### Scaling up
1. New pods are created 
2. Schedule to Nodes with available resources

### Scaling down
1. Reduces number of Pods

### Service
1. Service has an integrated load-balancer distributing network traffic to all Pods of an exposed deployment
2. Service monitors running Pods using endpoints

### Rolling updates
1. Rolling updates without downtime

```
kubectl get deployments
kubectl scale $DEPLOYMENT_NAME
kubectl get pods -o wide
```

# Update

Rolling Updates
Continuous Integration
Continuous Delivery

### Rolling
1. Allow deployments' update with zero downtime by incrementally update Pods instances
2. New Pods will be scheduled on Nodes with available resources
3. Multiple intances required
4. Service will load-balance the traffic to available Pods (available to application users)
5. Rolling updates allow:
..1. Promote application from one environment to another
..2. Rollback to previous version
..3. Continuous Integration and Continuous Delivery with zero downtime

```
kubectl set image [deployment-name][image-name] 
curl host:$NODE_PORT (32474)
kubectl rollout status deployments/kubernetes-bootcamp
kubectl rollout undo deployments/kubernetes-bootcamp
```
