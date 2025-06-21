<!-- Load Balancer -->
# What is a LoadBalancer service?

In AKS (Azure Kubernetes Service), a LoadBalancer service is used to:

* Expose your app to the internet
* Distribute traffic across Pods
* Give your app a public IP address

<!-- How it work -->
## How it Work
1. You have an app running in Pods.
2. You create a Service of type ```LoadBalancer```.
3. Kubernetes asks Azure to create a cloud load balancer.
4. Azure gives it a public IP address.
5. All traffic to that IP is automatically sent to your app’s Pods.

<!-- Real World example -->
## Real-world example: 
Imagine your app is a restaurant.
* Pods are the kitchen staff.
* The LoadBalancer is the front door with a host managing customers.
* It makes sure customers (users) are sent to available staff (Pods) without overloading anyone.

<!-- Creating Load Balancer seervice -->
## Create a LoadBalancer service example:
```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: LoadBalancer
  selector:
    app: myapp
  ports:
  - port: 80
    targetPort: 80
```

Then apply it:
```
kubectl apply -f my-service.yaml
```

Check the external IP:
```
kubectl get service my-service
```
It may take a minute to get the public IP.
