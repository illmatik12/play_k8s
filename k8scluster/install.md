git clone https://github.com/justmeandopensource/kubernetes 




# kubernetes Install 

* kmaster : 192.168.33.20
* kworker : 192.168.33.21



## Prerequsites 
### /etc/hosts/
192.168.33.20 kmaster.example.com kmaster 
192.168.33.21 kworker.example.com kworker 



### disable firewall
systemctl disable firewalld; systemctl stop firewalld


### disable swap
swapoff -a; sed -i '/swap/d' /etc/fstab

### disable SELinux 
setenforce 0
sed -i --follow-symlinks 's/^SELINUX=enforcing/SELINUX=disabled/' /etc/sysconfig/selinux


### update sysctl 
cat >>/etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sysctl --system


### install docker 
yum install -y yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install -y docker-ce-19.03.12 
systemctl enable --now docker


# kubernetes Setup 
## Add yum repository
cat >>/etc/yum.repos.d/kubernetes.repo<<EOF
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
        https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

## install kubernetes components 
yum install -y kubeadm-1.18.5-0 kubelet-1.18.5-0 kubectl-1.18.5-0

## Enable and Start kubelet service
systemctl enable --now kubelet


# On Master


kubeadm token create --print-join-command


kubeadm init --apiserver-advertise-address=192.168.33.20 --pod-network-cidr=192.168.1.0/16


```bash
Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.168.33.20:6443 --token 09ih7w.lg9nhk1s25rqju6x \
    --discovery-token-ca-cert-hash sha256:6f17524cce6386ee19c26fc0ff0f660291ed2f94a26bc16c54d60a06fb6c18e1 
[root@kmaster vagrant]# 
```

## Deploy calico network 
kubectl --kubeconfig=/etc/kubernetes/admin.conf create -f https://docs.projectcalico.org/v3.14/manifests/calico.yaml

## Workernod join cluster
kubeadm join 192.168.33.20:6443 --token mqyre9.rktcxtzugwpgwkd5     --discovery-token-ca-cert-hash sha256:6f17524cce6386ee19c26fc0ff0f660291ed2f94a26bc16c54d60a06fb6c18e1


## Check cluster status 

kubectl get nodes 
kubectl get componentstatus
kubectl cluster-info
kubectl version --short
kubectl get all -o wide

kubectl -n kube-system get all 


# install multi master

kubeadm init --control-plane-endpoint="172.16.16.100:6443" --upload-certs --apiserver-advertise-address=172.16.16.101 --pod-network-cidr=192.168.0.0/16


* master join시 apiserver-advertise-address 추가 

kubeadm join 172.16.16.100:6443 --token 6cjqxc.voutp75lde76iati \
    --discovery-token-ca-cert-hash sha256:b2abd30a72e65beb4e88efb59918cc58e727159280a103774ea5d7472ca3b0cb \
    --control-plane --certificate-key d77601c6645f5617b7fe15bc3c2b19d851f8ccc1d9e2cc73ea91d96ca865b7dc --apiserver-advertise-address=172.16.16.102

  kubeadm join 172.16.16.100:6443 --token 54oh3u.l1t4vtccutyvg03l \
    --discovery-token-ca-cert-hash sha256:b2abd30a72e65beb4e88efb59918cc58e727159280a103774ea5d7472ca3b0cb \
    --control-plane --certificate-key d77601c6645f5617b7fe15bc3c2b19d851f8ccc1d9e2cc73ea91d96ca865b7dc --apiserver-advertise-address=172.16.16.102





## Deploy calico network  to other master 
kubectl --kubeconfig=/etc/kubernetes/admin.conf create -f https://docs.projectcalico.org/v3.14/manifests/calico.yaml




#
kubeadm config images pull


#

kubeadm join 172.16.16.100:6443 --token bqt0xq.u7kq4nhl58yvzbe0 \
    --discovery-token-ca-cert-hash sha256:de4fa881e25a367ebf0ccf65ab6a33a84dda45c84badef418a929704c637b69b \
    --control-plane --certificate-key 3e7a3f5ecb3addba7ade6a2bef7344605dc716add60fd86b10e246e3352a0e61 --apiserver-advertise-address=172.16.16.102

Please note that the certificate-key gives access to cluster sensitive data, keep it secret!
As a safeguard, uploaded-certs will be deleted in two hours; If necessary, you can use
"kubeadm init phase upload-certs --upload-certs" to reload certs afterward.

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 172.16.16.100:6443 --token bqt0xq.u7kq4nhl58yvzbe0 \
    --discovery-token-ca-cert-hash sha256:de4fa881e25a367ebf0ccf65ab6a33a84dda45c84badef418a929704c637b69b 



kubeadm token create --print-join-command


# install local kubeconfig 
mkdir -p $HOME/.kube                         
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# install host kubeconfig 

mkdir ~/.kube 
scp root@172.16.16.101:/etc/kubernetes/admin.conf $HOME/.kube/config 






# 