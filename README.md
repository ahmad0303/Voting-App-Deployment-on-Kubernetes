# Voting Application Deployment on Kubernetes

This project demonstrates the deployment of a simple voting application on Kubernetes. The application consists of multiple microservices, including a voting app, a result app, a Redis database, a PostgreSQL database, and a worker service.

## Table of Contents
1. [Overview](#overview)
2. [Components](#components)
3. [Prerequisites](#prerequisites)
4. [Deployment Steps](#deployment-steps)
5. [Accessing the Application](#accessing-the-application)
6. [Cleanup](#cleanup)
7. [Contributing](#contributing)
8. [License](#license)

---

## Overview

The voting application allows users to vote between two options and displays the results in real-time. The application is composed of the following microservices:
- **Voting App**: Frontend for users to cast their votes.
- **Result App**: Frontend to display voting results.
- **Redis**: In-memory data store for temporary vote storage.
- **PostgreSQL**: Persistent database for storing vote results.
- **Worker**: Background service to process votes and store them in PostgreSQL.

---

## Components

### 1. **Voting App**
- **Image**: `kodekloud/examplevotingapp_vote:v1`
- **Service**: Exposed on NodePort `30004`.
- **Port**: 80
- **Deployment File**: `voting-app-deploy.yml`

### 2. **Result App**
- **Image**: `kodekloud/examplevotingapp_result:v1`
- **Service**: Exposed on NodePort `30005`.
- **Port**: 80
- **Deployment File**: `result-app-deploy.yml`

### 3. **Redis**
- **Image**: `redis`
- **Service**: Exposed on port `6379`.
- **Deployment File**: `redis-deploy.yml`

### 4. **PostgreSQL**
- **Image**: `postgres`
- **Environment Variables**:
  - `POSTGRES_USER`: `postgres`
  - `POSTGRES_PASSWORD`: `postgres`
- **Service**: Exposed on port `5432`.
- **Deployment File**: `postgres-deploy.yml`

### 5. **Worker**
- **Image**: `kodekloud/examplevotingapp_worker:v2`
- **Port**: 80
- **Deployment File**: `worker-app-deploy.yml`

---

## Prerequisites

Before deploying the application, ensure you have the following:
1. **Kubernetes Cluster**: A running Kubernetes cluster (e.g., Minikube, Kind, or a cloud-based cluster).
2. **kubectl**: Kubernetes command-line tool installed and configured.
3. **Docker**: Docker installed to pull container images (if not using a pre-configured cluster).

---

## Deployment Steps

1. **Clone the Repository**:
```bash
git clone <repository-url>
cd <repository-folder>
```

2. **Deploy the Application**:
   Apply the Kubernetes manifests to deploy the application:
```bash
kubectl apply -f postgres-deploy.yml
kubectl apply -f postgres-service.yml
kubectl apply -f redis-deploy.yml
kubectl apply -f redis-service.yml
kubectl apply -f voting-app-deploy.yml
kubectl apply -f voting-app-service.yml
kubectl apply -f result-app-deploy.yml
kubectl apply -f result-app-service.yml
kubectl apply -f worker-app-deploy.yml
```

3. **Verify Deployment**:
   Check the status of the pods and services:
```bash
kubectl get pods
kubectl get services
```

---

## Accessing the Application

### Voting App:
Open your browser and navigate to:
```
http://<cluster-ip>:30004
```
Replace `<cluster-ip>` with your Kubernetes cluster's IP address.

### Result App:
Open your browser and navigate to:
```
http://<cluster-ip>:30005
```

---

## Cleanup
To delete the deployed resources, run:
```bash
kubectl delete -f postgres-deploy.yml
kubectl delete -f redis-deploy.yml
kubectl delete -f voting-app-deploy.yml
kubectl delete -f result-app-deploy.yml
kubectl delete -f worker-app-deploy.yml
```

---

## Contributing

Contributions are welcome! If you'd like to improve this project, please:
- Fork the repository.
- Create a new branch for your feature or bugfix.
- Submit a pull request.

---

## License

This project is licensed under the MIT License. See the LICENSE file for details.

