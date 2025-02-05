# Implementing Role-Based Access Control (RBAC) with Kubernetes

This guide demonstrates how to set up a Kubernetes cluster, define roles and role bindings to enforce application separation, and test role enforcement within the Kubernetes environment. This setup ensures that users and applications have only the permissions they need, effectively controlling access to resources in your cluster.

---

## Setup Instructions

### 1. **Setup Minikube**
Refer to the `scripts/minikube` setup in the repository.

### 2. **Setup kubectl**
Refer to the `scripts/kubectl` setup in the repository.

### 3. **Setup Docker**
Refer to the `scripts/docker` setup in the repository.

### 4. **Start the Minikube Cluster**
Run the following command to start the Minikube cluster:
```bash
minikube start --memory=6144 --cpus=4 --driver=docker --force

kubectl create namespace dev
kubectl create namespace prod
# create the role files & role bindings

vi dev-role.yaml 
# see the repo for its content 
kubectl apply -f dev-role.yaml -n dev
vi dev-rolebinding.yaml 
# see the repo for its content 
kubectl apply -f dev-rolebinding.yaml -b dev 

# If you want to create the roles that you want to apply in all namespaces use cluster role below
vi clusterrole-amdin.yaml 
# see the repo for its content 
kubectl apply -f clusterrole-admin.yaml 
vi clusterrolebinding.yaml 
# see the repo for its content 
kubectl apply -f clusterrolebinding.yaml

# Test whether the roles are working as defined
kubectl auth can-i get pods --as=dev-user -n dev
kubectl auth can-i delete pods --as=dev-user -n dev
kubectl create deployment nginx --image=nginx -n dev --as=dev-user

# The output of above will be as defined in the roles
```
