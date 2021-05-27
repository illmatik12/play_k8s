# Deploying to Kubernetes cluster using Jenkins

## Using Jenkins using docker images 

docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -v jenkins_data:/var/jenkins_home jenkins/jenkins:lts 


## Continuos Integration / Continuous Delivery 

* Local docker registry 
docker run -d p 5000:5000 --restary-always --name registry -v docker:/var/lib/registry registry:o
t

