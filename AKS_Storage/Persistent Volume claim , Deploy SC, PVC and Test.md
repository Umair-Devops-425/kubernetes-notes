<!-- Step 1: Create a StorageClass (Local Volume for Minikube) -->
# Step 1: Create a StorageClass (Local Volume for Minikube)
```
# storageclass.yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: local-storage
provisioner: kubernetes.io/no-provisioner
volumeBindingMode: WaitForFirstConsumer
```
This uses ```no-provisioner``` to simulate manual disk management (local path).

<!-- Create a PersistentVolume (PV) and PersistentVolumeClaim (PVC) -->
# Step 2: Create a PersistentVolume (PV) and PersistentVolumeClaim (PVC)
# pv-pvc.yaml
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: local-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: local-storage
  hostPath:
    path: "/mnt/data"  # Make sure this exists in Minikube
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: local-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: local-storage
```

Create the ```/mnt/data``` directory inside Minikube:
```
minikube ssh
sudo mkdir -p /mnt/data
sudo chmod 777 /mnt/data
exit
```

<!-- 🧪 Step 3: Create a Test Pod -->
# Step 3: Create a Test Pod
```
# pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pod
spec:
  containers:
    - name: app
      image: busybox
      command: ["sleep", "3600"]
      volumeMounts:
        - name: test-volume
          mountPath: /data
  volumes:
    - name: test-volume
      persistentVolumeClaim:
        claimName: local-pvc
```

<!-- 🚀 Step 4: Apply Everything and Test -->
# Step 4: Apply Everything and Test
1. Apply all manifests:
```
kubectl apply -f storageclass.yaml
kubectl apply -f pv-pvc.yaml
kubectl apply -f pod.yaml
```

2. Exec into the pod and write a file:
```
kubectl exec -it test-pod -- sh
echo "Hello from PVC" > /data/hello.txt
cat /data/hello.txt
```

If you see:
```
Hello from PVC
```
✅ Your volume is working!


| Component | Purpose |
|-----------|---------|
| StorageClass | Defines how volumes are created (in this case, manually) |
| PersistentVolume | Actual storage (simulated with a folder on host) |
| PersistentVolumeClaim | User's request for storage |
| Pod | Uses the PVC to mount and write to the disk |