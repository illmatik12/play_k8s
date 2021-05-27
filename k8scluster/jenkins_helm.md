# Running Jenkins in kubernetes cluster Helm 

## deploy nfs provisioner 

kubectl create -f rbac.yaml -f class.yaml -f deployment.yaml


## helm 

### install jenkins 

helm search repo stable/jenkins
helm install repo stable/jenkins --> deprecated 

helm install repo stable/jenkins --value /tmp/jenkins.values

https://github.com/jenkinsci/helm-charts


helm install demo-jenkins stable/jenkins -f /tmp/values.yaml