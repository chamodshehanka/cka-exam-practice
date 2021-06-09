# Pods and Containers

```
kubectl run deployment busybox --image=busybox --restart=Never -o yaml --dry-run=client
```

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

```yaml
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

```yaml
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


## Self healing pods with restart policies

### Restart Policies
Always: This is the default policy. Container will be restarted if it crashes or unhealthy.
OnFailure: Container will be restarted only if it crashes.
Never: Container will never be restarted.

Always Restart Policy Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: always-restart-pod
  labels:
    name: always-restart-pod
  namespace: dev
spec:
  restartPolicy: Always # No need to provide this in yaml since this is the default
  containers:
  - name: busybox
    image: busybox
    command: ['sh', '-c', 'sleep 10']

```

On Failure Restart Policy Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: onfailure-restart-pod
  labels:
    name: onfailure-restart-pod
  namespace: dev
spec:
  restartPolicy: OnFailure
  containers:
  - name: busybox
    image: busybox
    command: ['sh', '-c', 'sleep 10'] # ['sh', '-c', 'sleep 10; THIS SHHSHHHSHHS']  this will failed the pod
```

## Mulicontainer Pod

A K8s Pod which has more than 1 container.

__Keep Containers seprarated unless they need to share the same resources__

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
  labels:
    name: multi-container-pod
  namespace: dev
spec:
  containers:
  - name: nginx
    image: nginx
  - name: redis
    image: redis
  - name: couchbase
    image: couchbase
```

Sidecar

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: sidecar-pod
  labels:
    name: sidecar-pod
  namespace: dev
spec:
  containers:
  - name: busybox
    image: busybox
    command:
      - sh
      - -c
      - while true; do echo logs data > /output/output.log; sleep 5; done
    volumeMounts:
      - name: shared-volume
        mountPath: /output  
  - name: sidecar
    image: busybox
    command:
      - sh
      - -c
      - tail -f /input/output.log
    volumeMounts:
      - name: shared-volume
        mountPath: /input
  volumes:
    - name: shared-volume
      emptyDir: {}
```

To Read the sidecar logs
```
kubectl logs sidecar-pod -c sidecar -n dev
```
busybox is writing the log and sidecar is reading the log.

## Init Containers

Init containers are used to perform initialization before the main container is started.

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: init-pod
  labels:
    name: init-pod
  namespace: dev
spec:
  containers:
  - name: nginx
    image: nginx
  initContainers:
    - name: delay
      image: busybox
      command: ["sleep", "3000"]
```

