<!-- YAML Basics -->
# What Is YAML?
* YAML is a human-readable data format used for configuration.
* It’s used by Kubernetes to define the desired state of objects.

<!-- YAML Syntax -->
## Basic YAML Syntax Rules
| Rule | Example |
|------|---------|
| Key-value pairs | ```name: myapp``` |
| Indentation matters | Use spaces, not tabs (2 or 4 spaces are common) |
| Lists start with a dash | ```- name: nginx``` |
| Colons separate key and value | ```image: nginx:1.25``` |
| Strings can be unquoted or quoted | ```image: "nginx:1.25"``` |
| Hierarchy is represented with indentation | nested values go under the key |\\\

<!-- Sample Structure: Kubernetes Pod YAML -->
# Sample Structure: Kubernetes Pod YAML
Let’s look at a simple ```Pod``` YAML and break it down:
```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  labels:
    app: myapp
spec:
  containers:
    - name: nginx-container
      image: nginx:1.25-alpine
      ports:
        - containerPort: 80
```
<!-- Explanation -->
### Explanation:
| Field | Description |
|-------|-------------|
| ```apiVersion``` | Tells Kubernetes which version of the API to use (e.g. v1) |
| ```kind``` | Type of resource (```Pod```, ```Service```, etc.) |
| ```metadata``` | Info like ```name```, ```labels```, etc. |
| ```spec``` | What the Pod should contain/do |
| ```containers``` | A list ```(-)``` of container definitions |
| ```image``` | Docker image to use |
| ```ports``` | Exposed container ports (inside the Pod) |