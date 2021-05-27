# dashboard 
* https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

kubectl -n kube-system get all



```
root@kmaster1:~# kubectl -n kubernetes-dashboard get all
NAME                                             READY   STATUS              RESTARTS   AGE
pod/dashboard-metrics-scraper-7b59f7d4df-jv8mb   1/1     Running             0          54s
pod/kubernetes-dashboard-74d688b6bc-ftk6c        0/1     ContainerCreating   0          56s

NAME                                TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/dashboard-metrics-scraper   ClusterIP   10.104.22.135   <none>        8000/TCP   56s
service/kubernetes-dashboard        ClusterIP   10.110.159.81   <none>        443/TCP    57s

NAME                                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/dashboard-metrics-scraper   1/1     1            1           55s
deployment.apps/kubernetes-dashboard        0/1     1            0           56s

NAME                                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/dashboard-metrics-scraper-7b59f7d4df   1         1         1       54s
replicaset.apps/kubernetes-dashboard-74d688b6bc        1         1         0       56s
root@kmaster1:~# 
```

# port forwarding 
```
kubectl -n kubernetes-dashboard port-forward kubernetes-dashboard-74d688b6bc-ftk6c 8000:8443
```




kubectl -n kubernetes-dashboard edit svc kubernetes-dashboard
```
  - nodePort: 32323
    port: 443
    protocol: TCP
    targetPort: 8443
```

kubectl -n kubernetes-dashboard describe sa kubernetes-dashboard

Name:                kubernetes-dashboard
Namespace:           kubernetes-dashboard
Labels:              k8s-app=kubernetes-dashboard
Annotations:         <none>
Image pull secrets:  <none>
Mountable secrets:   kubernetes-dashboard-token-9krmx
Tokens:              kubernetes-dashboard-token-9krmx
Events:              <none>




kubectl -n kubernetes-dashboard describe secret kubernetes-dashboard-token-9krmx

```
root@kmaster1:~/work# kubectl -n kubernetes-dashboard describe secret kubernetes-dashboard-token-9krmx
Name:         kubernetes-dashboard-token-9krmx
Namespace:    kubernetes-dashboard
Labels:       <none>
Annotations:  kubernetes.io/ser:wvice-account.name: kubernetes-dashboard
              kubernetes.io/service-account.uid: 95e04fc4-5a06-4675-99b3-e5c846a870d7

Type:  kubernetes.io/service-account-token

Data
====
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IjlwYUI4cWVVOG9wVWZJbnFyLUtJTngxeHZWYTFTemppVjduNVhSWk9uMVEifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZC10b2tlbi05a3JteCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJrdWJlcm5ldGVzLWRhc2hib2FyZCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6Ijk1ZTA0ZmM0LTVhMDYtNDY3NS05OWIzLWU1Yzg0NmE4NzBkNyIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlcm5ldGVzLWRhc2hib2FyZDprdWJlcm5ldGVzLWRhc2hib2FyZCJ9.cntCJUsHMD_rV3cbSrFhT_lfoW-rxClYXmP1_KOnJYRHHWhO9bKVKAwzxYA9L0puhk04SHc6wbjQ1fH_62aUqWurEQr5NFQdhrk17ehRzaQAbOeMAkMGZm7wOYaBa-qnC8KrAbAX_dSrI_S8IKMjaaj2KuNkNZm5bVaLRP8oWtXDwGoZ_CrEPLo_QZn_cWyaMOOz_bf_Ugvgvsrtme9edUCxy-SojG9pn6nv5ApLPrRZ0ONDXoUTQVSu6SznZpfwMtgS_sNzI-YGG7KcFwke_8A0czyzgOoDXU0yJ_KHm1tXK_FuJ-LMnB0d-Xyqgj4X1merOxrqAEc0dVy53Vhpqg
ca.crt:     1066 bytes
namespace:  20 bytes
root@kmaster1:~/work# 
```

## admin 
kubectl -n kube-system describe sa dashboard-admin


kubectl -n kube-system describe secret dashboard-admin-token-xxg7c
```
root@kmaster1:~/work/kubernetes/dashboard# kubectl -n kube-system describe secret dashboard-admin-token-xxg7c
Name:         dashboard-admin-token-xxg7c
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: dashboard-admin
              kubernetes.io/service-account.uid: 7ac78e37-c521-4c67-982f-d885129d71ba

Type:  kubernetes.io/service-account-token

Data
====
ca.crt:     1066 bytes
namespace:  11 bytes
token:      eyJhbGciOiJSUzI1NiIsImtpZCI6IjlwYUI4cWVVOG9wVWZJbnFyLUtJTngxeHZWYTFTemppVjduNVhSWk9uMVEifQ.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkYXNoYm9hcmQtYWRtaW4tdG9rZW4teHhnN2MiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC5uYW1lIjoiZGFzaGJvYXJkLWFkbWluIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQudWlkIjoiN2FjNzhlMzctYzUyMS00YzY3LTk4MmYtZDg4NTEyOWQ3MWJhIiwic3ViIjoic3lzdGVtOnNlcnZpY2VhY2NvdW50Omt1YmUtc3lzdGVtOmRhc2hib2FyZC1hZG1pbiJ9.ZP6lAix2M3LrfZRiP0VrnOjq0PrqMr99P0y6hOf1mGA0XFc7azFVinKAiGcdA0trYwicInkh6k4Jd88wd38Zp6r4DBM5KDRl-OcpwIRS09UOwdM_w8pxl7t7b38kLYOkOjC99VBq-L6mt8bZvj_BEd-2oms6UR3JTKxrUbEbjlYFVZoFt3fYSLpqGWpuHAorBq9dAHhKZDYtVw4Wpsvjy5xGgsRSngeymvplGWKVRNnzj-DPdZsT5yVIMF8N4AMkNt9Svpd3lFXLjv83qvHZB4RwxxjwY2-C63hJVSzYjkaF9EBkotc-wSb8YlTcTmpPTUvf6vXWP9qALaj2KCLpAQ


```






