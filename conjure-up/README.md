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


sudo lxd init

use localhost lxc controller

juju bootstrap localhost lxd

lxc network set lxdbr0 ipv6.address none

juju bootstrap localhost lxd


### Conjure up kubernetes on localhost

conjure-up kubernetes

or Headless mode

conjure-up canonical-kubernetes localhost

# Destroy

conjure-down

juju switch
juju destroy-controller $controllername --destroy-all-models 
