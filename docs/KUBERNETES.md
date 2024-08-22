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
  replicas: 10
  selector:
    matchLabels:
      app: posts-service
  template:
    metadata:
      labels:
        app: posts-service
    spec:
      containers:
      - name: posts-service-container
        image: techtest/posts-service:v1
        ports:
        - containerPort: 3000
        resources:
          requests:
            cpu: "500m"
            memory: "512Mi"
          limits:
            cpu: "1000m"
            memory: "1Gi"
```

hpa.yaml

```bash
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: posts-service-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: posts-service
  minReplicas: 10
  maxReplicas: 50
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 20
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 30
```
