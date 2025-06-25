<!-- Kubernetes Secrets -->
# What are Kubernetes Secrets?
Kubernetes Secrets are a way to store sensitive data (like passwords, API keys, tokens, etc.) safely inside a Kubernetes cluster.

<!-- Why do we need Secrets? -->
## Why do we need Secrets?
- Imagine your app needs a database password. If you put it directly in your code or configuration file, it's not safe—anyone who sees the code can see the password.

- So instead, you store that password in a Kubernetes Secret. Kubernetes keeps it hidden and gives it to the app when needed.

<!-- What can you store in a Secret? -->
## What can you store in a Secret?
Examples:
- Database passwords
- API keys
- SSH keys
- TLS certificates
- Any sensitive string value


## Create a Secret:
1. You create it using a YAML file or ```kubectl``` command.
```
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: abcd123   # base64 encoded 'myuser'
  password: abcdrtuhsji==  # base64 encoded 'mypassword'
```

2. Mount the Secret:
- As environment variables in your container.
- As files inside your container (mounted as a volume).

<!-- Example Use -->
## Example Use
Let's say your app needs a database login:
- You store the ```username``` and ```password``` in a Secret.
- Your app Pod reads those values from the Secret (either via env variables or files).
- Now your app can connect to the database—securely.

<!-- Are Secrets secure? -->
## Are Secrets secure?
- They are base64-encoded by default (not encrypted, just hidden).
- But Kubernetes can encrypt them at rest if configured.
- Access to Secrets is controlled by RBAC (Role-Based Access Control).


