# cloud-developer
content for Udacity's cloud developer nanodegree

#Git Repo Link Project#3 
https://github.com/jaypatel0113/cloud-developer/tree/master/course-03/exercises

#Refactoring the application
Code has been refactor in user, feed, frontend and reverseproxy to handle traffic for port 8080 for backend

#Container: Docker Image for Udagram application Microservice

1) Build Images: docker-compose -f docker-compose-build.yaml build --parallel
2) Push Images to Docker Hub: docker-compose -f docker-compose-build.yaml push

#Kubernetes:
After Installing kubernetes, Terraform follow commands below to setup kubernetes cluster in AWS

#Setting up AWS credentials

export AWS_ACCESS_KEY_ID=...
export AWS_SECRET_ACCESS_KEY=...

#Setting Infrastructure on AWS

Downloads AWS plugins:

    terraform init

Terraform can use plan command to see what changes will be made:

    terraform plan


Provision the infrastructure:

    terraform apply --auto-approve

Generate JSON output file to be used to install kubernetes using terraform output:
    
    terraform output -json >tf.json
    

Install kubernetes using install command and providing the configuration file, above create terraform output file tf.json

    kubeone install config.yaml -t tf.json  
    
xport the KUBECONFIG variable environment variable (ClusterName is demo)

    export KUBECONFIG=$PWD/demo-kubeconfig 

Check master nodes are running

    kubectl get nodes

#Configuration of the Application:

Apply secrets to the cluster

    kubectl apply -f k8s/aws-secret.yaml 
    kubectl apply -f k8s/env-secret.yaml 

Apply config maps to the cluster

    kubectl apply -f k8s/env-configmap.yaml
    
Apply services

    kubectl apply -f k8s/backend-feed-service.yaml 
    kubectl apply -f k8s/backend-user-service.yaml 
    kubectl apply -f k8s/frontend-service.yaml 
    kubectl apply -f k8s/reverseproxy-service.yaml 

Deploy Pods to kubernetes using Docker container images

    kubectl apply -f k8s/backend-feed-deployment.yaml
    kubectl apply -f k8s/backend-user-deployment.yaml 
    kubectl apply -f k8s/frontend-deployment.yaml
    kubectl apply -f k8s/reverseproxy-deployment.yaml  

To check secrets, configmaps, services use below commands:

    kubectl get secrets
    kubectl get configmaps
    kubectl get services
    
To check pods deployed successfully:

    kubectl get pod

port-forward to forward services when you have multiple nodes running and load balancing to the port is handled by services

    kubectl port-forward services/reverseproxy 8080:8080
    kubectl port-forward service/frontend 8100:8100
    
