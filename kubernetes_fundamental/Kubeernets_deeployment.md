<!-- Kubernetes Deployment -->
# What is a Kubernetes Deployment?
A Deployment is a higher-level Kubernetes object that:
* Manages your Pods automatically
* Ensures your app is always available
* Supports scaling, updating, and rollback

<!-- Simple Terms -->
### In Simple Terms:
* A Deployment is like a smart manager for your Pods.
* It uses ReplicaSets under the hood but gives you more features.

<!-- Why to use -->
## Why use a Deployment (vs just a ReplicaSet)?
| Feature | ReplicaSet | Deployment |
|---------|------------|------------|
| Self-healing Pods | ✅ Yes | ✅ Yes |
| Rolling updates | ❌ No | ✅ Yes |
| Rollback support | ❌ No | ✅ Yes |
| Scaling up/down | ✅ Manual | ✅ Easy |
| Best practice | ❌ No | ✅ Yes |

<!-- Example using Yaml -->
## Example: Deployment YAML
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80
```

Apply it: 
```
kubectl apply -f nginx-deployment.yaml
```

Check Pods:
```
kubectl get pods
```

<!-- Comman Deployment -->
## Common Deployment Commands
Update image (rolling update):
```
kubectl set image deployment/nginx-deployment nginx=nginx:1.22
```

Rollback to previous version:
```
kubectl rollout undo deployment/nginx-deployment
```

Scale up/down:
```
kubectl scale deployment/nginx-deployment --replicas=5
```

Check status:
```
kubectl rollout status deployment/nginx-deployment
```

## TL;DR:
* Deployment is the most common way to run apps in Kubernetes.
* Deployment is the most common way to run apps in Kubernetes.
* Deployment is the most common way to run apps in Kubernetes.