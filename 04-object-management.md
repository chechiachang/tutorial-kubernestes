Object management
===
### Key Words

Imperative commands
Imperative object configuration
Declarative object configuration

### Imperative Commands

Command

```
kubectl run nginx --image nginx
# The same
kubectl create deployment nginx --image nginx
```

1. Simple command
2. Single step

3. No integrate with change review processes
4. No audit trail associated with changes
5. No provide source of records
6. No template

### Imperative object configuration

YAML or JSON

```
vim nginx.yaml
kubectl create -f nginx.yaml
kubectl delete -f nginx.yaml -f redis.yaml
kubectl replate -f nginx.yaml
```

1. Configuration can be stored in VCS, git
2. Configuration can integrate with processes reviewing changes before push
3. Configuration provide template
4. Configuration is simple and easy to understande

5. Require understanding of schema
6. Require to write YAML file
7. Works best on files not directory
8. Update must apply to configuration files, or changes be lost during the next deployment

### Declarative object configuration

```
kubectl apply -f configs/
kubectl apply -R -f configs/
```

1. Changes made to live objects are retained
2. Better support for operating on directory

3. Harder to debug
4. Diff create complex merge operations

# Managing Kubernetes Objects

### Imperative Command

Run: Create a new Deployment object to run Containers in one or more Pods
Expose: Create a new Service object to load balance traffic across Pods
Autoscale: Create a Autoscaler object to automatically scale a controller, such as a Deployment

