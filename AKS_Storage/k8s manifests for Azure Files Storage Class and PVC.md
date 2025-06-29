<!-- manifests for Azure Files Storage Class and PVC -->
# manifests for Azure Files Storage Class and PVC
Let’s walk through Kubernetes (K8s) manifests that use Azure Files for storage. This usually involves:
- A StorageClass (optional for dynamic provisioning).
- A PersistentVolumeClaim (PVC) to request storage.
- (Optional) A Pod that uses the PVC.

<!-- 1. StorageClass for Azure Files (Dynamic Provisioning) -->
1. StorageClass for Azure Files (Dynamic Provisioning)
This is used when you want Kubernetes to automatically create an Azure File Share for you.
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: azurefile-sc
provisioner: kubernetes.io/azure-file
parameters:
  skuName: Standard_LRS  # or Premium_LRS for SSD
reclaimPolicy: Retain
volumeBindingMode: Immediate

```

<!-- 2. PersistentVolumeClaim (PVC) -->
2. PersistentVolumeClaim (PVC)
The PVC requests a volume from the StorageClass above.
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azurefile-pvc
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: azurefile-sc
  resources:
    requests:
      storage: 5Gi
```
- ```ReadWriteMany``` allows multiple pods to mount the volume.
- ```5Gi``` is the size you're requesting.

<!-- 3. Example Pod using the PVC -->
3. Example Pod using the PVC
```
apiVersion: v1
kind: Pod
metadata:
  name: azurefile-demo-pod
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - name: azure
          mountPath: /mnt/azure
  volumes:
    - name: azure
      persistentVolumeClaim:
        claimName: azurefile-pvc
```
This mounts the Azure File Share to ```/mnt/azure``` inside the container.

