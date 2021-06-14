# Services

Service - Kubernetes Services provide a way to expose an application running as a set of Pods. 
They provide a way to expose a set of Pods as a single logical service.

ServiceType - Every service has a type. The ServiceType determines how and where the service will expose your application. There are 4 service types:
* ClusterIP - Exposes a service which is only accessible from within the cluster.
* NodePort - Exposee a service via a static port on every node's IP, Expose to outside.
* LoadBalancer - Exposes a service via the cloud provider's load balancer.
* ExternalName - Exposes a service via an arbitrary name. (Outside the CKA scope)

Create a deployment to try out a Service.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: svc-example
  name: svc-example
  namespace: dev
spec:
  replicas: 3
  selector:
    matchLabels:
      app: svc-example
  strategy: {}
  template:
    metadata:
      labels:
        app: svc-example
    spec:
      containers:
      - image: nginx
        name: nginx
        ports:
          - containerPort: 80
status: {}
```

Then create the ClusterIP service.

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: svc-cluster-ip
  name: svc-cluster-ip
  namespace: dev
spec:
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
  selector:
    app: svc-cluster-ip
  type: ClusterIP
```


Create the NodePort service.
```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: svc-nodeport
  name: svc-nodeport
  namespace: dev
spec:
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30080
  selector:
    app: svc-nodeport
  type: NodePort
```

# Discovering Services with DNS

K8s DNS assign DNS names to Services, allowing application within the cluster to easily locate them.

A service's fully qualified domain name has the following format:
```
service-name.namespace.svc.cluster.cluster-domain.example
```

The default cluster domain is `cluster.local`.

