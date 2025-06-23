<!-- Load Balancer Service -->
# Below is a ready-to-apply LoadBalancer Service that will expose the testpod
Below is a ready-to-apply LoadBalancer Service that will expose the ```testpod```
```
apiVersion: v1
kind: Service
metadata:
  name: testpod-lb
spec:
  type: LoadBalancer       # Minikube will simulate a cloud LB
  selector:
    app: myapp             # must match the Pod’s label
  ports:
    - port: 80             # Service port (stable, external)
      targetPort: 80       # Pod containerPort
```

<!-- How to use -->
## How to use it
save as service.yaml
```
kubectl apply -f service.yaml
```

In a separate terminal, start the tunnel so Minikube can assign an external IP
```
minikube tunnel
```

back in your main terminal
```
kubectl get svc testpod-lb
```

Note the EXTERNAL-IP column, then:
```
curl http://<EXTERNAL-IP>
```