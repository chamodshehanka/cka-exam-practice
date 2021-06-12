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

## Using Static Pods

Static Pod - A pod that is managed by directly by the kubelet on a node, not by the K8s API server. They can run even if there is no K8s API server present.

Mirror Pod - Kubelet will create a mirror Pod for each static Pod. Mirror allow you to see the status of the static Pod via K8s API. But you cannot change or manage them via API.

```
sudo vi /etc/kubernetes/manifests/my-static-poy.yaml
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-static-pod
  labels:
    name: my-static-pod
  namespace: dev
spec:
  containers:
  - name: nginx
    image: nginx
```

Wait till kubernetes detect the file change or simply restart the kubelet
```
sudo systemctl restart kubelet
```

