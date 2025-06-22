<!-- Step-1 -->
# Step 1
Step 1: Create the Deployment
Create a file called ```nginx-deployment.yaml```:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
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
        image: nginx
        ports:
        - containerPort: 80
```

Apply it: 
```
kubectl apply -f nginx-deployment.yaml
```

Check Pods:
```
kubectl get pods -l app=nginx
```



<!-- Step 2 -->
# Step 2
Step 2: Expose the Deployment with a LoadBalancer Service
Create a file called ```nginx-service.yaml```:
```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
```

Apply it:
```
kubectl apply -f nginx-service.yaml
```

Get the external IP:
```
kubectl get service nginx-service
```

Test:
```
curl http://<EXTERNAL-IP>
```

<!-- Step-3 -->
# Step 3
Step 3: Scale Up the Deployment
```
kubectl scale deployment nginx-deployment --replicas=5
```

Check:
```
kubectl get pods -l app=nginx
```
✅ You should see 5 Pods.

<!-- Step 4 -->
# Step 4
Step 4: Scale Down the Deployment
```
kubectl scale deployment nginx-deployment --replicas=2
```

Check again:
```
kubectl get pods -l app=nginx
```
✅ Only 2 Pods should be running now.


<!-- Step 5 -->
# Step 5
Step 5 (Optional): Clean Up
```
kubectl delete -f nginx-deployment.yaml
kubectl delete -f nginx-service.yaml
```

