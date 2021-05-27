# Dynamic NFS provisioning 


## setup nfs server 

### create nfs directory 

```bash
sudo mkdir -p /srv/nfs/kubedata 
sudo chown nobody: /srv/nfs/kubedata 

sudo pacman -S nfs-utils 

sudo systemctl start nfs-server

vi /etc/exports 
/srv/nfs/kubedata *(rw,sync,no_subtree_check,no_root_squash,no_all_squash,insecure)

sudo exportfs -rav 

sudo exportfs -v 


mount -t nfs 172.20.10.2:/srv/nfs/kubedata /mnt

```

## cluster setup 

* kubernetes/yamls/nfs-provisioner 

```
kubectl get clusterrole,clusterrolebinding,role,rolebinding

kubectl create -f rbac.yaml
```

* storage class


kubectl get storageclass       
kubectl create -f class.yaml

* Deployment
kubectl create -f deployment.yaml


* pv,pvc create 

* pvc 
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc1
spec:
  storageClassName: managed-nfs-storage 
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 500Mi

```

* pv
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox
spec:
  volumes:
  - name: host-volume
    persistentVolumeClaim:
      claimName: pvc1
  containers:
  - image: busybox
    name: busybox
    command: ["/bin/sh"]
    args: ["-c", "sleep 600"]
    volumeMounts:
    - name: host-volume
      mountPath: /mydata
```

kubectl get pv,pvc 


ls -al /srv/nfs/kubedata 


## pod test 
```
kubectl create -f 4-busybox-pv-hostpath.yaml


kubectl exec -it busybox -- sh 

touch /mydata/file 
```


## delete persistent volume 
kubectl delete pvc --all