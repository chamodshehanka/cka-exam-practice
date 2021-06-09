# Deployments

Use imperative command to generate fast
```
kubectl create deployment my-deployment --image=nginx --replicas 3 -o yaml --dry-run=client > my-deployment.yaml
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: my-deployment
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-deployment
  strategy: {}
  template:
    metadata:
      labels:
        app: my-deployment
    spec:
      containers:
      - image: nginx
        name: nginx
        ports:
          - containerPort: 80
        resources: {}
status: {}
```

## Scaling Deployments

1. Editing yaml and apply 
2. Kubernetes Scale Command
```
kubectl scale deployment.v1.apps/my-deployment --replicas=3
```

## Rolling Pods
Gradually replacing new pods for old pods

```
kubectl edit deployment my-deployment -n dev 
```

```
kubectl rollout status deployment.v1.apps/my-deployment -n dev
```

Another way
```
kubectl set image deployment.v1.apps/my-deployment -n dev nginx=nginx:latest
```

To view the history
```
kubectl rollout history deployment.v1.apps/my-deployment -n dev
```

To undo // --to-revision is optional
```
kubectl rollout undo deployment.v1.apps/my-deployment -n dev --to-revision=2 
```