# Kubernetes Object Management

Declarative - Define objects using data structures such as YAML or JSON.
Imperative - Define objects using kubectl commands and flags.

```
kubectl create deployment nginx --image=nginx
```


```
kubectl create deployment nginx --image=nginx --dry-run -o yaml
```