# helm 

## helm 
- chart 
- repo 



## Commands 
```
helm install 
helm status
helm search 
helm repo update 
helm upgrade
helm rollback 
helm delete

```
## install 

* install scripts 
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh
```

* install binary 
```
tar zxvf helm-.tar.gz
cd linux-amd64
sudo mv helm /usr/local/bin/
```
helm version --short 

## Create service account tiller
kubectl -n kube-system create serviceaccount tiller

## rollbinding

kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller


## helm init 
* do not need v3 
helm init --service-account tiller 

## helm add repo

* no longer available 
helm repo add stable https://kubernetes-charts.storage.googleapis.com/

* works fine 
helm repo add stable https://charts.helm.sh/stable
