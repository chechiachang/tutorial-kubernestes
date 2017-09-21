Get Started 
==

Install kudeadm
 - [x] Install Docker [Official]()
 - [x] Install kubectl
 - [x] Install kubelet and kubeadm
 - [x] Initializing your master

### Pre-install setting

On CentOS

systemctl stop firewalld && systemctl disable firewalld
setenforce 0
vi /etc/selinux/config 
SELINUX=disabled

### Install docker

curl -sSL https://get.docker.com/ | sh

### Install Kubectl

curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl \
&& chmod +x kubectl \
&& sudo mv kubectl /usr/local/bin


### Install kubelet and kubeadm

apt-get update && apt-get install -y apt-transport-https \
&& curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add - \
&& cat <<EOF >/etc/apt/sources.list.d/kubernetes.list 
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF 
apt-get update \
&& apt-get install -y kubelet kubeadm

### Initializing your master

kubeadm init --pod-network-cidr=10.244.0.0/16

To start using your cluster, you need to run (as a regular user):

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  http://kubernetes.io/docs/admin/addons/

### Join your master

On another machine with kebeadm, run as root:

  kubeadm join --token token ip:port

On the master machine, check:

  kubectl get no
  kubectl get cs

### Install Pod Network

Run:

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel-rbac.yml

Check:

kubectl get pods --all-namespaces

### Install a test application

Check:

kubectl get namespace

Run:

kubectl create namespace my-test

kubectl -n my-test get svc front-end

### Trouble shooting

hostnamectl --set-hostname server1
hostnamectl --set-hostname server2

kubeadm init
kubenetes certification is signed to hostname

