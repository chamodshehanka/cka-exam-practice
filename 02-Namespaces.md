# Namespaces

### Get Namespaces
```
kubectl get namespaces

kubectl get ns
```

### Important ns

1. default
2. kube-system - ns to keep system components

### Get Pods with namespace
```
kubectl get pods --namespace <NAME_SPACE>
```

### Get All pods from all namespaces

```
kubectl get pods --all-namespaces
```