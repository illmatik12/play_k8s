# prometheus monitoring for kubernetes

* prometheus nodeport : 32322 
* Grafana nodeport : 32323 

* nfs ip : 10.131.162.66

## prometheus configuration 
helm inspect values repo stable/prometheus

helm install stable/prometheus --name prometheus --values /home/supaflow/work/kubernetes/yamls/nfs-provisioner/prometheus/prom_values.yaml --namespace prometheus --> not working 

```
helm install stable/prometheus --name prometheus --values /home/supaflow/work/kubernetes/yamls/nfs-provisioner/prometheus/prom_values.yaml --namespace prometheus


default storageclass 정의 함 

kubectl create namespace prometheus 
helm install prometheus stable/prometheus  --values /home/supaflow/work/kubernetes/yamls/nfs-provisioner/prometheus/prom_values.yaml --namespace prometheus


kubectl -n prometheus get all
```

## Grafana Configuration 


helm inspect values  stable/grafana > grafana_values.yaml 


kubectl create namespace  grafana
helm install grafana stable/grafana --values /home/supaflow/work/kubernetes/yamls/nfs-provisioner/prometheus/grafana_values.yaml --namespace grafana

kubectl get all -n grafana


# 참고 
https://gruuuuu.github.io/cloud/l-helm-basic/#