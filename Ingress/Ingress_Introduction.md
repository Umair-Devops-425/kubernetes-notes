<!-- What is Ingress? -->
# What is Ingress?
Ingress is like a road or gateway that lets external users (like someone on the internet) access your apps running inside a Kubernetes cluster.

<!-- In simple words: -->
## In simple words:
- Your apps are hidden inside Kubernetes.
- Ingress helps to expose them to the outside world safely and efficiently.
- It lets you use a single IP or domain to reach multiple apps.

<!-- What can Ingress do? -->
## What can Ingress do?
- Route traffic based on URL paths or hostnames.
- Use HTTPS (SSL) to secure traffic.
- Control access (e.g., block or allow users).
- Combine multiple services under one domain (like ```example.com/shop```, ```example.com/blog```).

<!--  How it works (basic idea): -->
## How it works (basic idea):
- You deploy an Ingress Controller (a special app that understands Ingress rules).
- You define Ingress rules (in YAML) that say:
    - “If someone goes to ```/blog```, send them to the blog app.”
    - “If the request is for ```shop.example.com```, send it to the shopping app.

<!-- Example (very simple): -->
```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /app1
        pathType: Prefix
        backend:
          service:
            name: app1-service
            port:
              number: 80
```
This says: “When someone visits ```example.com/app1```, send them to ```app1-service.```”