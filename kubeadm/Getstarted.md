Get Started 
==

Install kudeadm
 - [x] Install Docker [Official]()
 - [x] Install kubectl
 - [ ] Install kubelet and kubeadm
   - [ ] 
 - [ ] Initializing your master


### Install docker

curl -sSL https://get.docker.com/ | sh

### Install Kubectl

curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin


### Install kubelet and kubeadm

apt-get update && apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm

### Initializing your master

kubeadm init

