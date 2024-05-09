# Kubernets-Docker-GuestApp

The following commands are for the Linux Operating System 

# Objectives:
1) to create a kubernetes cluster
2) to deploy the web application 
3) to manage the pods using kubernetes


## Prerequites:
Docker need to be installed


## Step1:
 We will use the Kind to create the Kuberntes cluster : Run the following commands to install Kind 
 
```bash 
[ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.22.0/kind-linux-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind
```

## Step 2:  Create a Yaml file and copy the below script

```bash
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
  extraPortMappings:
  - containerPort: 30949
    hostPort: 80
  - containerPort: 30950
    hostPort: 443

```

## Step 3: Run the command to create the cluster

```bash
kind create cluster --config=kind-config.yaml
```

## Install kubectl command line

```bash
sudo snap install kubectl --classic
```

## Step 4: Apply the Redis Deployment from the redis-leader-deployment.yaml file

```bash
Kubectl apply -f redis-leader-deployment.yaml

kubectl get pods 

```
You should see pods up running

## Step 5:Apply the Redis Service from the following redis-leader-service.yaml file:

```bash
Kubectl apply -f redis-leader-service.yaml
```

Query the list of Services to verify that the Redis Service is running:

```bash
kubectl get service
```

## Step 6: Setup Redis followers

```bash
Kubectl apply -f redis-follower-deployment.yaml
```

Verify that the two Redis follower replicas are running by querying the list of Pods:

```bash
kubectl get pods
```

## Step 7: Creating the Redis follower service

```bash
kubectl apply -f  redis-follower-service.yaml
```

Query the list of Services to verify that the Redis Service is running:

```bash
kubectl get service
```

## Step 8: Set up and Expose the Guestbook Frontend

```bash
kubectl apply -f  frontend-deployment.yaml
```

Query the list of Pods to verify that the three frontend replicas are running:

```bash
kubectl get pods -l app=guestbook -l tier=frontend
```

## Step 9:Creating the Frontend Service

```bash
kubectl apply -f frontend-service.yaml
```

Query the list of Services to verify that the frontend Service is running:

```bash
kubectl get services
```

## Step 10: Run the following command to forward port 8080 on your local machine to port 80 on the service.

```bash
kubectl port-forward svc/frontend 8080:80
```

## The response should be similar to this:

Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80

load the page http://localhost:8080 in your browser to view your guestbook.

