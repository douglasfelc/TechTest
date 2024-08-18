# Kubernetes

[Click here](../README.md) to return to general documentation.

Steps to build and deploy containerized service on Kubernetes.

1. Write Dockerfile
2. Build Docker Image
3. Push Image to a Registry (Example: Docker Hub)
4. Create Kubernetes Manifests
5. Deploy to Kubernetes


## Manifests

deployment.yaml

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: posts-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: posts-service
  template:
    metadata:
      labels:
        app: posts-service
    spec:
      containers:
      - name: posts-service
        image: techtest/posts-service:v1
        ports:
        - containerPort: 3000
```
