# Hello World Kubernetes Application

This project demonstrates deploying a simple "Hello World" web application using Docker and Kubernetes with KIND (Kubernetes IN Docker).

## Project Structure

```
hello-world-devops/
├── app/
│   └── index.html          # Simple HTML page displaying "Hello world from kubernetes"
├── demo/                   # Demo materials
├── docker/
│   └── Dockerfile          # Instructions to build the nginx-based Docker image
├── k8s/
│   ├── deployment.yaml     # Kubernetes deployment configuration with 2 replicas
│   └── service.yaml        # Kubernetes NodePort service exposing port 30080
├── kind-config.yaml        # KIND cluster configuration with port mapping
└── README.md               # This file
```

## Prerequisites

- Docker
- kubectl
- KIND (Kubernetes IN Docker)
- PowerShell (for Windows users)

## Step-by-Step Guide

### 1. Create a KIND Cluster

Create a local Kubernetes cluster using KIND with custom port mapping:

```bash
kind create cluster --name hello-world-v1-cluster --config kind-config.yaml
```

This creates a single-node cluster with the control-plane node exposing port 30080, which will be used to access our application.

### 2. Build the Docker Image

Build the Docker image containing a simple nginx server with our custom HTML page:

```bash
docker build -t hello-world:latest -f docker/Dockerfile .
```

### 3. Load the Image into KIND

Since KIND creates a separate Docker network, we need to load our local image into the KIND cluster:

```bash
kind load docker-image hello-world:latest --name hello-world-v1-cluster
```

### 4. Deploy the Application to Kubernetes

Apply the Kubernetes manifests to create the deployment and service:

```bash
kubectl apply -f k8s/
```

This creates:
- A deployment with 2 replicas of our hello-world container
- A NodePort service that exposes the application on port 30080

### 5. Verify Deployment

Check the status of your deployment and pods:

```bash
kubectl get deployments
kubectl get pods
kubectl get services
```

You should see the hello-world deployment with 2 pods running and the service exposing port 30080.

## Accessing the Application

Once deployed, you can access the application by navigating to:

```
http://localhost:30080
```

This will display the "Hello world from kubernetes" message.

