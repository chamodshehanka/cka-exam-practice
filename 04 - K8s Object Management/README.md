# Kubernetes Object Management

Declarative - Define objects using data structures such as YAML or JSON.
Imperative - Define objects using kubectl commands and flags.

```
kubectl create deployment nginx --image=nginx
```

Use dry run to see what would be created.
```
kubectl create deployment nginx --image=nginx --dry-run -o yaml
```
To save dry run output to a file.
```
kubectl create deployment nginx --image=nginx --dry-run -o yaml > nginx.yaml
```

