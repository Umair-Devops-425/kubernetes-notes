<!-- What is a ConfigMap? -->
# What is a ConfigMap?
* A ConfigMap is used to store configuration data separately from your application code.
* Think of it like a settings file (like ```.env```, ```.ini```, or ```config.json```), but stored in Kubernetes.

<!-- Why use a ConfigMap? -->
## Why use a ConfigMap?
Imagine you have an app that needs:
* A database URL
* A feature flag
* A custom greeting message
You don’t want to hardcode these into your app. Instead:
* Store them in a ConfigMap
* Inject them into the Pod (as environment variables or files)
This makes your app easier to configure without changing code.

<!-- Example: Create a ConfigMap -->
## Example: Create a ConfigMap
YAML File: ```configmap.yaml```
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  APP_MODE: "production"
  GREETING: "Hello from ConfigMap"
```

<!-- Example: Use ConfigMap in a Pod -->
## Example: Use ConfigMap in a Pod
YAML: ```pod-with-configmap.yaml```
```
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
spec:
  containers:
    - name: app
      image: busybox
      command: ["sh", "-c", "env; sleep 3600"]
      env:
        - name: APP_MODE
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: APP_MODE
        - name: GREETING
          valueFrom:
            configMapKeyRef:
              name: my-config
              key: GREETING
```
This pod will print environment variables, and you’ll see:
```
APP_MODE=production
GREETING=Hello from ConfigMap
```

<!-- Apply and Test -->
## Apply and Test
```
kubectl apply -f configmap.yaml
kubectl apply -f pod-with-configmap.yaml
kubectl exec -it app-pod -- env
```

<!-- Alternative: Mount ConfigMap as a File -->
## Alternative: Mount ConfigMap as a File
You can also use a ConfigMap as a file:
```
volumeMounts:
  - name: config-vol
    mountPath: /etc/config
volumes:
  - name: config-vol
    configMap:
      name: my-config
```

Each key in the ConfigMap becomes a file inside ```/etc/config```

