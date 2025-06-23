<!-- ReplicaSet and LoadBalancer Service -->
# Below is a complete, ready-to-run example that puts everything in one file:
* ReplicaSet named ```rs-nginx``` with 3 replicas
* LoadBalancer Service named ```rs-nginx-svc``` that exposes the Pods on port 80
* Matching labels: ```app: nginx```, ```tier: frontend```

### Save the file as ```rs-lb.yaml```.
```
# ---------- ReplicaSet ----------
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: rs-nginx
  labels:
    app: nginx
    tier: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
      tier: frontend
  template:
    metadata:
      labels:
        app: nginx
        tier: frontend
    spec:
      containers:
        - name: nginx
          image: nginx:1.25-alpine
          ports:
            - containerPort: 80

---
# ---------- LoadBalancer Service ----------
apiVersion: v1
kind: Service
metadata:
  name: rs-nginx-svc
spec:
  type: LoadBalancer
  selector:
    app: nginx
    tier: frontend
  ports:
    - port: 80        # Stable external port
      targetPort: 80  # Pod containerPort
```

# Run and Test
1 — Apply
```
kubectl apply -f rs-lb.yaml
```

2 — Verify ReplicaSet & Pods
```
kubectl get rs
kubectl get pods -l app=nginx -o wide
```
You should see 3/3 replicas and three Pods.

3 — Start the Minikube LB tunnel
```
minikube tunnel
```
Keep this terminal open; it simulates a cloud load balancer.

4 — Get the Service’s external IP
In a second terminal:
```
kubectl get svc rs-nginx-svc
```

Look for the EXTERNAL-IP column, e.g.:
```
NAME           TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)   AGE
rs-nginx-svc   LoadBalancer   10.96.203.42   192.168.49.2   80/TCP    25s
```

5 — Test
```
curl http://<EXTERNAL-IP>
```
You should receive the default NGINX welcome HTML.

6 — Self-healing demo (optional)
Delete one Pod and watch it return:
```
kubectl delete pod <one-of-the-pod-names>
kubectl get pods -l app=nginx --watch   # Ctrl-C when the replacement appears
```

7 — Clean up
```
kubectl delete -f rs-lb.yaml
```
(Then ```Ctrl-C``` the ```minikube tunnel``` window.)

<!-- What you Learned -->
# What you learned
* ReplicaSet guarantees the replica count & auto-heals failed Pods.
* LoadBalancer Service gives one stable IP; in Minikube it’s simulated via minikube tunnel.
* End-to-end workflow: apply → verify → test → observe self-healing → cleanup.


