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

#### Record Command
To record that used to make a change.
```
kubectl scale deployment nginx --replicas=3 --record
```
Then use describe to see the change and records.

## K8s RBAC
Roles and ClusterRoles are both Kubernetes objeects that define set of permissions.

Roles - Define permissions within a particular namespace.
ClusterRoles - Define permissions across all namespaces(Entire cluster).

```
kubectl create role pod-reader --verb=get --resource=pods --dry-run -o yaml > pod-reader-role.yaml
```

RoleBinding and ClusterRoleBinding are objects that bind roles to users and groups.

RoleBinding - Binds a role to a user or group.
ClusterRoleBinding - Binds a role to a user or group across all namespaces.

```
kubectl create rolebinding pod-reader-role-binding --role=pod-reader --dry-run -o yaml > pod-reader-role-binding.yaml
```

### Service Account
In K8s, a service account is an account used my containers processes within Pods to authenticate with the K8s API.
If your pods need to communicate with the API, you need to create a service account.

We can manage access control for service accounts using ClusterRoleBindings or ClusterRole.

```
kubectl create serviceaccount my-service-account --dry-run -o yaml > my-service-account.yaml
```
or 
```
kubectl create sa my-service-account --dry-run -o yaml > my-service-account.yaml
```

Then create a role binding to bind that permission with role binding

```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: sa-rolebinding
  namespace: dev
subjects:
- kind: ServiceAccount
  name: my-service-account
  namespace: dev
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: pod-reader
```

### Kubernetes Metrics Server
Kubernetes provides a metrics server that can be used to monitor the cluster.

#### Kubernetes top

With kubernetes top, you can see the resource usage of the cluster. Kubectl top supports flags like ```--sort``` and ```--selector```

```
kubectl top pods --sort-by=cpu --selector=app=nginx
```

1. Apply Metrics Server in Control Plane Node
```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

2. Verify Metrics Server Installation
```
kubectl get --raw /apis/metrics.k8s.io/
```

3. Get Pods metrics
```
kubectl top pod --all-namespaces
```
```
kubectl top pod --sort-by=cpu --all-namespaces
```
```
kubectl top pod --sort-by=cpu -n dev
```
```
kubectl top node
```