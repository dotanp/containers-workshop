
# 🏗️ Kubernetes Lab: Exploring and Deploying Microservices on Minikube

## 📌 **Lab Overview**
Welcome to this Kubernetes lab! In this exercise, you'll work with **Pods, ReplicaSets, Deployments, and Services** in a **Minikube** cluster. You'll also **explore** the cluster to find specific information about its setup.

---
## 📖 **Scenario: Online Bookstore Service**
Your company is launching a **microservice-based online bookstore**, and you are responsible for deploying the backend service to Kubernetes.

---
## 🔍 **Part 1: Exploring the Cluster**
Before deploying applications, let's gather information about the cluster.

### 🔹 **1. Check Node Information**
📌 Find the **IP address** of the Minikube node:
```sh
minikube ip
```
📌 Get details about the nodes in the cluster:
```sh
kubectl get nodes -o wide
```
📌 What **role** does the node have? *(Hint: Look under the ROLES column.)*

### 🔹 **2. Check Cluster Namespaces**
📌 List all namespaces in the cluster:
```sh
kubectl get namespaces
```
📌 Find your **current namespace**:
```sh
kubectl config view --minify --output 'jsonpath={..namespace}'
```
*(If blank, you're in the `default` namespace.)*

### 🔹 **3. Check How Many Nodes Are Available**
📌 Run:
```sh
kubectl get nodes
```
*(Minikube typically runs as a single-node cluster, but this may change in custom setups.)*

---
## 🚀 **Part 2: Deploying the Bookstore Backend**
Now, let’s deploy a simple backend service using Kubernetes.

### 📝 **4. Create and Edit Kubernetes Objects Using Dry-Run Mode**
Before applying objects directly, let's generate their YAML manifests using `--dry-run=client` and save them for later editing.

#### **Pod Dry-Run Example**
📌 Generate a Pod definition file:
```sh
kubectl run bookstore --image=nginx --port=80 --dry-run=client -o yaml > bookstore-pod.yaml
```
🛠 Edit `bookstore-pod.yaml` and customize it as needed before applying it.

#### **ReplicaSet Dry-Run Example**
📌 Generate a ReplicaSet definition file:
```sh
kubectl create rs bookstore-replicaset --image=nginx --replicas=3 --dry-run=client -o yaml > bookstore-replicaset.yaml
```
🛠 Modify `bookstore-replicaset.yaml` as needed before applying it.

#### **Deployment Dry-Run Example**
📌 Generate a Deployment definition file:
```sh
kubectl create deployment bookstore-deployment --image=nginx --replicas=3 --dry-run=client -o yaml > bookstore-deployment.yaml
```
🛠 Edit `bookstore-deployment.yaml` to customize it.

#### **Service Dry-Run Example**
📌 Generate a Service definition file:
```sh
kubectl expose deployment bookstore-deployment --port=80 --target-port=80 --type=NodePort --dry-run=client -o yaml > bookstore-service.yaml
```
🛠 Edit `bookstore-service.yaml` before applying it.

### ✅ **5. Apply Kubernetes Objects from YAML Files**
Once you've modified the files, apply them:
```sh
kubectl apply -f bookstore-pod.yaml
kubectl apply -f bookstore-replicaset.yaml
kubectl apply -f bookstore-deployment.yaml
kubectl apply -f bookstore-service.yaml
```

### 🔍 **6. Verify Deployments**
📌 Check the status of the deployed objects:
```sh
kubectl get pods
kubectl get replicasets
kubectl get deployments
kubectl get services
```

---
## 📊 **7. Read Logs and Values from Pods**
Once the pods are running, you may want to check their logs and environment variables.

#### 📌 **Reading Pod Logs**
```sh
kubectl logs <pod-name>
```
*(If the pod has multiple containers, specify the container name:)*
```sh
kubectl logs <pod-name> -c <container-name>
```

#### 📌 **Reading Environment Variables from a Running Pod**
```sh
kubectl exec <pod-name> -- printenv
```
*(To inspect a specific variable:)*
```sh
kubectl exec <pod-name> -- printenv | grep VARIABLE_NAME
```

#### 📌 **Executing a Shell Inside a Running Pod**
```sh
kubectl exec -it <pod-name> -- /bin/sh
```
*(Use `/bin/bash` if available.)*

---
## ❓ **Part 3: Questions for Exploration**
Now that you've deployed the bookstore service, answer the following:

1. **What is the current status of your pods?** *(Hint: `kubectl get pods`)*
2. **What is the name of the ReplicaSet managing the bookstore pods?** *(Hint: `kubectl get replicasets`)*
3. **Check which Deployment controls the ReplicaSet.** *(Hint: `kubectl get deployments`)*
4. **How many namespaces exist in your cluster?** *(Hint: `kubectl get namespaces`)*
5. **Find detailed information about the bookstore pod.** *(Hint: `kubectl describe pod bookstore`)*
6. **What is the external URL of your bookstore service?** *(Hint: `minikube service bookstore-service --url`)*
7. **Check the logs of your bookstore pod.** *(Hint: `kubectl logs <pod-name>`)*
8. **List all environment variables inside the bookstore pod.** *(Hint: `kubectl exec <pod-name> -- printenv`)*

---
## 🗑️ **Cleanup (Optional)**
To remove everything:
```sh
kubectl delete -f bookstore-service.yaml
kubectl delete -f bookstore-deployment.yaml
kubectl delete -f bookstore-replicaset.yaml
kubectl delete -f bookstore-pod.yaml
```

---
## 🎉 **Conclusion**
You have successfully deployed a simple **online bookstore backend** using Kubernetes **Pods, ReplicaSets, Deployments, and Services**! You've also explored the cluster, checked logs, and examined environment variables.

💡 Would you like to enhance this lab by adding a frontend service or configuring an Ingress? 🚀


