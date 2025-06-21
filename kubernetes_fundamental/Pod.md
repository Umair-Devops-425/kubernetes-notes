# What is a Pod in AKS?
A Pod is the smallest unit in AKS (Azure Kubernetes Service) that runs your application.
* It contains one or more containers (like Docker containers).
* These containers share the same IP address, storage, and network.
* Pods make sure your app runs smoothly inside Kubernetes.

A Pod is like a box that holds one or more containers. If containers are workers, the Pod is their office—they work together and share tools (network/storage).

## Why use Pods?
* Kubernetes doesn't manage containers directly—it manages Pods.
* Pods help with scaling, updating, and recovering your apps easily.

You can create a Pod using a simple YAML file or directly from the command line.

<!-- Step-1-->
### Option A: Using a YAML file
1. Create a file called ```mypod.yaml```:
```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mycontainer
    image: nginx
    ports:
    - containerPort: 80
```
<!-- Step-2 -->
2. Apply the YAML file to create the Pod:
```
kubectl apply -f mypod.yaml
```
### Option B: Directly from the command line
```
kubectl run mypod --image=nginx --port=80
```
Check if the Pod is running
```
kubectl get pods
```
You should see mypod listed and its status (e.g., Running).

<!-- Step-3 -->
3. Delete the Pod
```
kubectl delete pod mypod
```
This will remove the Pod from your cluster.