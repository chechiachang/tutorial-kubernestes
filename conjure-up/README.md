Kubernetes with conjure-up
===

[Deploy local kubernetes with lxc](https://kubernetes.io/docs/getting-started-guides/ubuntu/local/)

# Prerequisite

### conjure-up

sudo apt-get update
sudo apt-get install snapd
sudo snap install conjure-up

check
 - sudo snap list
 - juju status

### lxc

sudo apt install lxc

check
 - lxc list

# Deploy 

```
sudo lxd init

# use localhost lxc controller
juju bootstrap localhost lxd

# disable controller ipv6
lxc network set lxdbr0 ipv6.address none

juju bootstrap localhost lxd

conjure-up kubernetes
```

