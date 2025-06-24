<!-- AKS -->
# What is AKS?
AKS (Azure Kubernetes Service) is a platform where you can run your containers (like small, packaged apps). It's like a smart system that automatically manages your applications.

<!-- Why do we need storage in AKS? -->
## Why do we need storage in AKS?
Containers are temporary – if they restart, any files they stored vanish. So we need storage that keeps data safe and permanent, even if the container stops.

<!-- What is Azure Disk? -->
## What is Azure Disk?
Azure Disk is like a virtual hard drive you can attach to your AKS containers. It stores files just like a normal disk on your laptop or server.

<!-- How Azure Disk works with AKS -->
## How Azure Disk works with AKS
1. You create a disk (e.g., 100 GB).
2. You attach it to a container (actually to a Kubernetes pod).
3. The pod can then read and write files to that disk.
4. If the pod restarts, the disk is still there – data is safe.
5. If the pod moves to another node, Azure can re-attach the disk.

<!-- Key Points -->
## Key Points
* Azure Disk is best for single-writer pods (only one pod writes to it at a time).
* It’s fast and good for things like databases.
* For shared access by many pods, use Azure Files instead.

<!-- Summary -->
## Summary
Think of Azure Disk like a USB drive:
* You plug it into your container.
* It keeps data safe.
* It's managed by Azure so you don’t have to worry about hardware.
