


kubectl run my-nginx --image=nginx

kubectl scale --replicas=2 deployment/my-nginx 


# demo 
kubectl create deploy nginx --image nginx 


# kubeadm add addtional master node 

## token create 
kubeadm token list 

kubeadm init phase upload-certs --upload-certs

```
root@kmaster1:~/work# kubeadm init phase upload-certs --upload-certs
W1125 04:34:06.940695   82403 configset.go:348] WARNING: kubeadm cannot validate component configs for API groups [kubelet.config.k8s.io kubeproxy.config.k8s.io]
[upload-certs] Storing the certificates in Secret "kubeadm-certs" in the "kube-system" Namespace
[upload-certs] Using certificate key:
046f4a6fcd6c4f7c7e69ccc3cc0d5aca546a2c9626860a59839eaf71482251e2
root@kmaster1:~/work# 


```

kubeadm token create --certificate-key 046f4a6fcd6c4f7c7e69ccc3cc0d5aca546a2c9626860a59839eaf71482251e2 --print-join-command

```
kubeadm join 172.16.16.100:6443 --token v4f2y0.qb453o2mh9vxp7pr     --discovery-token-ca-cert-hash sha256:de4fa881e25a367ebf0ccf65ab6a33a84dda45c84badef418a929704c637b69b     --control-plane --certificate-key 046f4a6fcd6c4f7c7e69ccc3cc0d5aca546a2c9626860a59839eaf71482251e2 --apiserver-advertise-address=172.16.16.103

```

