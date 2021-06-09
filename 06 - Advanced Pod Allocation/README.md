# Advanced Pod Allocation

Scheduling - Assigning pods to nodes' kubelet can run them.
Scheduler - Control plane component that handles scheduling.

Before that it checks 

* Resource requests and available node resources.
* Configs that affect scheduling using node labels.

Node Selector - We can configure a **nodeSelector** for your pods to limit which Node(s) the Pod can be scheduled on.
Node selctor use node lables to filter nodes.


Using node name
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: nginx
spec:
    containers:
    - name: nginx
      image: nginx
    nodeSelector: 
      mylabel: myvalue
```

nodeName
```yaml
apiVersion: v1
kind: Pod
metadata:
    name: nginx
spec:
    containers:
    - name: nginx
      image: nginx
    nodeName: k8s-worker1
```

To label a node

```
kubectl label nodes <NODE_NAME> <LABEL_NAME>=<LABLE_VALUE>
```
```
kubectl label nodes gke-chamod-default-pool-7c9d38c3-89wt special=true
```

Then the pod looks like below
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nodeselector-pod
  labels:
    name: nodeselector-pod
  namespace: dev
spec:
  nodeSelector:
    special: 'true'
  containers:
  - name: nginx
    image: nginx
    resources:
      limits:
        memory: "128Mi"
        cpu: "500m"
```

Using Node name
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nodename-pod
  labels:
    name: nodename-pod
  namespace: dev
spec:
  nodeName: gke-chamod-default-pool-7c9d38c3-89wt
  containers:
  - name: nginx
    image: nginx
    resources:
      limits:
        memory: "128Mi"
        cpu: "500m"
```

## Using DaemonSets

DaemonSet - Automatically runs a copy of a Pod on each node.
also run copy of Pod on new nodes as they are added to the cluster.

### DaemonSets and Scheduling
We can combine DaemonSet with Scheduling

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: my-daemonset
  namespace: dev
spec:
  selector:
    matchLabels:
      app: my-daemonset
  template:
    metadata:
      labels:
        app: my-daemonset
    spec:
      containers:
        - name: nginx
          image: nginx   
```

