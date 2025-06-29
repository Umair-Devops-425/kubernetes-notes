<!-- What is Ingress (in general)? -->
# What is Ingress (in general)?
Think of Ingress as the gatekeeper for your applications running inside Kubernetes.
- It receives requests (like from a web browser) from the outside world.
- It decides which app inside your cluster should handle the request.
- It's like a traffic manager for your cluster.
So instead of opening a port for every app (which is messy), you use Ingress to cleanly route traffic.

<!-- Ingress in Minikube (your local setup) -->
## Minikube is like your own tiny Kubernetes lab on your computer. To use Ingress:
Steps (Simplified):
1. Start Minikube with Ingress addon:
```
minikube addons enable ingress
```
2. Deploy your app + Ingress config (YAML files).
3. Use Minikube's IP or ```minikube tunnel``` to access the app.

<!-- Example: -->
Example:  You visit ```http://myapp.local```, and the Ingress sends that to your app running in Minikube.

<!-- Ingress in AKS (Azure Kubernetes Service) -->
# Ingress in AKS (Azure Kubernetes Service)
AKS is Kubernetes in the cloud, managed by Azure.
In AKS, you often use a cloud-native Ingress controller, usually NGINX or Azure Application Gateway.

Steps (Simplified):
1. Install an Ingress controller (like NGINX) using Helm:
```
helm install nginx ingress-nginx/ingress-nginx
```
2. Deploy your apps + Ingress config.
3. Azure will give you a public IP, and Ingress will route traffic to the correct app.

<!-- Example -->
Example:
You visit ```https://shop.mycompany.com```, and AKS routes that request through Ingress to your shopping app.

