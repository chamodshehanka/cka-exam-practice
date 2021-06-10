# Networking 

The Kubernetes network model is a set of standards that define how networking between Pods bahaves.

There are a variety of different implementations of this model - including the Calico network plugin, which we have been using throught this course. 

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: busybox-dnstest
  name: busybox-dnstest
  namespace: dev
spec:
  containers:
  - name: busybox-dnstest
    image: radial/busyboxplus:curl
    command: ['sh', '-c', 'while true; do sleep 3600; done']
---
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx-dnstest
  name: nginx-dnstest
  namespace: dev
spec:
  containers:
  - name: nginx-dnstest
    image: nginx
    ports:
      - containerPort: 80
```

Find nginx IP Address 
```
kubectl get pods nginx-dnstest -o wide -n dev
```

Exec Curl to nginx
```
kubectl exec busybox-dnstest -n dev -- curl 10.28.1.23 
```

---
Get nginx pod IP
```
kubectl get pod nginx-dnstest -n dev -o wide
```

Do ns lookup from busy box
```
kubectl exec busybox-dnstest -n dev -- nslookup 10-28-1-23.dev.pod.cluster.local
```