<!-- What is a StorageClass? -->
# What is a StorageClass?
A StorageClass in Kubernetes defines how storage should be created. It tells Kubernetes things like:
* What type of disk to use (e.g., Premium SSD, Standard HDD).
* How big it should be (when used with PersistentVolumeClaims).
* What kind of performance is expected.
In AKS, it connects to Azure Disks or Azure Files behind the scenes.

<!-- Simple StorageClass YAML for Azure Disk -->
## Simple StorageClass YAML for Azure Disk
Here’s a basic StorageClass YAML that uses Azure Disk:
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: azure-disk-sc
provisioner: kubernetes.io/azure-disk
parameters:
  storageaccounttype: Premium_LRS
  kind: Managed
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsumer
```

<!-- What does each part mean? -->
## What does each part mean?
What does each part mean?
| Field | Explanation |
|-------|-------------|
| apiVersion | Tells Kubernetes what version of the resource type you’re using. |
| kind: StorageClass | You're creating a StorageClass object. |
| metadata.name | The name you give this class (used in PVCs). |
| provisioner | This tells Kubernetes what type of storage to use. For Azure Disk, it's ```kubernetes.io/azure-disk``` |
| parameters.storageaccounttype | This defines the type of disk (e.g., ```Standard_LRS```, ```Premium_LRS```etc.)|
| parameters.kind | Use ```Managed``` to let Azure handle the disks for you. |
| reclaimPolicy | If set to ```Delete```, the disk is deleted when the PVC is deleted. Use Retain if you want to keep the data. |
| volumeBindingMode | ```WaitForFirstConsumer ```delays disk creation until a pod uses it — good for placing it close to the pod. |

<!-- Example: Use the StorageClass with a PVC -->
## Example: Use the StorageClass with a PVC
Here’s how you can use that StorageClass in a PersistentVolumeClaim (PVC):
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-disk-pvc
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: azure-disk-sc
  resources:
    requests:
      storage: 10Gi
```

<!-- What happens? -->
## What happens?
* Kubernetes looks for the azure-disk-sc StorageClass.
* It creates a 10 GB Azure Managed Disk (Premium).
* This PVC can then be mounted by a pod.

<!-- Example Pod using the PVC -->
## Example Pod using the PVC
```
apiVersion: v1
kind: Pod
metadata:
  name: azure-disk-pod
spec:
  containers:
    - name: app
      image: nginx
      volumeMounts:
        - mountPath: "/mnt/azure"
          name: azure-disk
  volumes:
    - name: azure-disk
      persistentVolumeClaim:
        claimName: azure-disk-pvc
```
This mounts the Azure Disk to /mnt/azure in the pod.

<!-- Summary -->
## Summary
* StorageClass defines the type of disk to create.
* PVC uses that class to ask for a specific amount of storage.
* Pods use PVCs to mount the disk and use it.