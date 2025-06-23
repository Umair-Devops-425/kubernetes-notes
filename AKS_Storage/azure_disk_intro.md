<!-- Introduction -->
# What Are Azure Disks?
* Azure Disks are block-level storage volumes managed by Azure.
* In AKS, they are commonly used as persistent storage for individual Pods.

<!-- Key Concepts -->
## Key Concepts
| Term | Description |
|------|-------------|
| Managed Disk | A virtual hard disk (VHD) managed by Azure. Types: Premium SSD, Standard SSD, Standard HDD, and Ultra SSD | 
| PersistentVolume (PV) | A K8s abstraction of a storage resource |
| PersistentVolumeClaim (PVC) | A user's request for storage (size, access mode, etc.) |
| StorageClass | Defines how volumes are provisioned (disk type, replication, etc.) |

<!-- Why to Use -->
## Why Use Azure Disks in AKS?
* ✅ Highly available and durable
* ✅ Managed by Azure (no manual provisioning)
* ✅ Great for stateful apps like databases
* ❌ Can be attached to only one Pod at a time (unless using Azure Disk CSI driver with shared disks)

<!-- Typical Architecture -->
## Typical Architecture
```
Pod → PVC → StorageClass → Azure Managed Disk
```

<!-- StorageClass -->
## StorageClass Example (Azure Disk)
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: azure-disk-sc
provisioner: disk.csi.azure.com
parameters:
  skuName: Premium_LRS      # or StandardSSD_LRS, UltraSSD_LRS, etc.
  kind: Managed
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsume
```

<!-- PVC -->
## PVC Example
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: azure-disk-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: azure-disk-sc
```

<!-- 🧩 Mount to Pod -->
## Mount to Pod
```
apiVersion: v1
kind: Pod
metadata:
  name: disk-demo
spec:
  containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - mountPath: "/mnt/data"
          name: disk-storage
  volumes:
    - name: disk-storage
      persistentVolumeClaim:
        claimName: azure-disk-pvc
```

<!-- Important Notes -->
## Important Notes
| Item | Details |
|------|---------|
| ReadWriteOnce (RWO) | Azure Disks can only be mounted to one node at a time unless using shared disks |
| Availability Zones | Make sure your disk and node pool are in the same zone |
| CSI Driver | Azure Disk uses the CSI driver (disk.csi.azure.com) by default in AKS |

<!-- Use Case -->
## ✅ Use Cases
* SQL databases (SQL Server, MySQL, PostgreSQL)
* MongoDB, Redis (if not using managed services)
* Apps that need dedicated, persistent storage per replica

<!-- 🔁 Cleanup -->
## When the Pod is deleted:
* If the PVC is deleted and ```reclaimPolicy: Delete```, the Azure Disk is deleted automatically.
* Otherwise, it stays in your Azure subscription as an orphaned resource (can be deleted manually).

<!--  -->