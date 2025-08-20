# Tasky Application (Minikube Local Testing)
#test
Tasky is a simple to-do list application built with Go. This guide provides instructions for setting up and running the application locally using Minikube, with a focus on simplicity and reliability for team testing and security tool evaluation.

## Prerequisites
- [Docker](https://docs.docker.com/get-docker/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## Project Structure
- `mongodb-storage.yaml`: PersistentVolume and PersistentVolumeClaim for MongoDB data
- `mongodb-deployment.yaml`: MongoDB Deployment and Service
- `tasky-config.yaml`: ConfigMap for application configuration
- `tasky-deployment.yaml`: Tasky app Deployment and NodePort Service
- `tasky-admin.yaml`: (Optional) ServiceAccount and ClusterRoleBinding for admin access

## Quick Start

### 1. Clone the Repository
```bash
git clone <your-repo-url>
cd tasky
```

### 2. Build the Docker Image
```bash
docker build -t your-docker-id/tasky:latest .
```
Replace `your-docker-id` with your Docker Hub username or a local registry name.

### 3. Load the Image into Minikube
```bash
minikube image load your-docker-id/tasky:latest
```

### 4. Start Minikube
```bash
minikube start
```

### 5. Deploy the Application
Apply the manifests in order:
```bash
kubectl apply -f mongodb-storage.yaml
kubectl apply -f mongodb-deployment.yaml
kubectl apply -f tasky-config.yaml
kubectl apply -f tasky-deployment.yaml
```

### 6. Access the Application
The easiest way is:
```bash
minikube service tasky-service
```
This will open the app in your browser at a temporary localhost URL.

Alternatively, you can access it at:
```
http://<minikube-ip>:30000
```
Find your Minikube IP with:
```bash
minikube ip
```

## Security Notice

This repository is intended for local testing and demonstration purposes only.
**Hardcoded secrets (such as database passwords) are present by design and should never be used in production environments.**

If you receive secret scanning alerts from GitHub or GitGuardian, you can safely mark them as "used for testing/demo" or "won't fix" for this repository.

## Cleaning Up
```bash
kubectl delete -f tasky-deployment.yaml
kubectl delete -f tasky-config.yaml
kubectl delete -f mongodb-deployment.yaml
kubectl delete -f mongodb-storage.yaml
```

## Troubleshooting
- If you see `ErrImagePull`, make sure you loaded the image into Minikube and that the image name in `tasky-deployment.yaml` matches.
- If you can't access the app via `<minikube-ip>:30000`, use `minikube service tasky-service` for a reliable local tunnel.
- MongoDB data is persisted in the Minikube VM via a hostPath volume.

---

This setup is designed for easy cloning and local testing by any team member. No Ingress or advanced networking is required—just Minikube, Docker, and kubectl!
