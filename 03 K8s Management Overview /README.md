# K8s Management Overview

Provide interface with K8s

- kubectl - K8s offical CLI tool
- kubeadm - Tools to create K8s cluster easily
- minikube - Local single node k8s cluster
- helm - pkg management tool for K8s Object
- kompose - Helps to transform docker-compose files into K8s Objects
- kustomize - Config management toop

## Safetly Draining a K8s Node

For the maintain purpose we can drain the node. Container running on the node will be gracefully terminated(Potentially rescheuled on another node)

```
kubectl drain <NODE_NAME>
```

### Ignoring DaemonSets
When draining a node, you may need to ignore daemonsets(pods that are tied to each node)
```
kubectl drain <NODE_NAME> --ignore-daemonsets
```

