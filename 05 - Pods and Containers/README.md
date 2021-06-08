# Pods and Containers

## Managing Application Configuration
When running an app on K8s, we may want to pass dynamic values to the app. <br/>
1. <b>ConfigMaps</b> to pass values to the app. ConfigMaps stores key-value pairs.
2. <b>Secret</b> to pass values to the app. Secret stores encrypted values.

We can pass ConfigMaps or Secret as <b>environment variables</b> or <b>Configuration Volumes</b> to the app. 
```
kubectl create configmap my-configmap --dry-run=client -o yaml > my-configmap.yaml
```

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-configmap
  namespace: dev
data:
  key1: Hello, world!
  key2: |
    Test 1
    Test 2
    Test 3
```

## Container Health

Liveness probe: to know when to restart the a container
Startup Probes: to know when a container application has started (eg: healthz)
Readiness probe: allow to determine whether or not a container is ready to receive traffic.

Liveness probes examples are below

```
apiVersion: v1
kind: Pod
metadata:
  name: liveness-pod
  labels:
    name: liveness-pod
  namespace: dev
spec:
  containers:
  - name: busybox
    image: busybox
    command: ['sh', '-c', 'while true; do sleep 3600; done']
    livenessProbe:
      exec:
        command: ["echo", "Hello, world!"]
      initialDelaySeconds: 5
      periodSeconds: 5 
```

with http 

```
apiVersion: v1
kind: Pod
metadata:
  name: liveness-pod-http
  labels:
    app: liveness-pod-http
  namespace: dev
spec:
  containers:
  - name: nginx
    image: nginx:latest
    livenessProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 5
```
Other probes yaml definition is same only difference is when the probe is executed.
