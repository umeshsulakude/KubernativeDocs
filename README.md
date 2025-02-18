# Kubernetes Crash Course

## Introduction to Kubernetes
Kubernetes is an open-source container orchestration tool developed by Google. It helps manage applications composed of multiple containers deployed across different environments, including physical machines, virtual machines, cloud, and hybrid infrastructures. Kubernetes automates container management tasks, making it easier to scale applications, ensure high availability, and recover from failures.

## Why Kubernetes?
- **Microservices Growth:** Managing hundreds or thousands of containers manually is challenging.
- **High Availability:** Ensures continuous application uptime.
- **Scalability:** Automatically scales applications based on demand.
- **Disaster Recovery:** Restores data and services in case of infrastructure failure.

## Kubernetes Architecture
### Cluster Setup
A Kubernetes cluster consists of:
- **Master Node:** Controls the cluster and manages its components.
- **Worker Nodes:** Execute application containers.

### Master Node Components
- **API Server:** Entry point for Kubernetes clients (CLI, UI, API calls).
- **Controller Manager:** Ensures the desired state of the cluster is maintained.
- **Scheduler:** Assigns workloads to nodes based on available resources.
- **ETCD Storage:** Stores cluster configuration and state data.

### Worker Node Components
- **Pods:** The smallest unit in Kubernetes, containing one or more containers.
- **Kubelet:** Ensures containers run in a given node.
- **Container Runtime:** Runs containers (e.g., Docker, containerd).
- **Kube Proxy:** Manages networking between services and pods.

## Kubernetes Core Components
- **Pods:** Basic unit of deployment, containing one or more containers.
- **Service:** Provides a stable IP address for a set of pods.
- **Ingress:** Manages external access to services via HTTP/HTTPS.
- **ConfigMap & Secret:** Manage configuration data and sensitive information.
- **Volume:** Provides persistent storage for containers.
- **Deployment & StatefulSet:** Manage application deployment and scaling.
- **Replica Sets:** Maintain the desired number of pod replicas.
- **API Server:** Central control plane for managing the cluster.

## Deploying a Web Application with MongoDB in Kubernetes
### Prerequisites
- A running Kubernetes cluster (Minikube for local development).
- Installed **kubectl** CLI.
- Dockerized web application and MongoDB.

### YAML Configuration
#### Web Application Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapp-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: webapp
  template:
    metadata:
      labels:
        app: webapp
    spec:
      containers:
      - name: webapp
        image: your-docker-image
        ports:
        - containerPort: 8080
```
#### Web Application Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: webapp-service
spec:
  type: NodePort
  selector:
    app: webapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 31000
```
#### MongoDB Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
      - name: mongodb
        image: mongo:latest
        ports:
        - containerPort: 27017
```
#### MongoDB Service
```yaml
apiVersion: v1
kind: Service
metadata:
  name: mongodb-service
spec:
  selector:
    app: mongodb
  ports:
    - protocol: TCP
      port: 27017
```

### Deploying the Application
Apply YAML configurations:
```sh
kubectl apply -f webapp-deployment.yaml
kubectl apply -f mongodb-deployment.yaml
```
Check the status of deployed resources:
```sh
kubectl get all
```
View details of a specific resource:
```sh
kubectl describe <resource> <name>
```

### Troubleshooting and Validation
- **Check logs of a pod:**
  ```sh
  kubectl logs <pod-name>
  ```
- **Get real-time log streaming:**
  ```sh
  kubectl logs -f <pod-name>
  ```
- **Access application externally:**
  ```sh
  minikube service webapp-service --url
  ```
- **Get cluster node IP:**
  ```sh
  kubectl get nodes -o wide
  ```

## Summary
This crash course covered:
- Kubernetes fundamentals and architecture.
- Core components like Pods, Services, Deployments, and ConfigMaps.
- Deployment of a web application with MongoDB.
- Useful **kubectl** commands for managing and troubleshooting Kubernetes clusters.

For further learning, refer to the [official Kubernetes documentation](https://kubernetes.io/docs/).

