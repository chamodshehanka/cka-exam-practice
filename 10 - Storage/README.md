# Storage

Container -> Persistent Volume Claim -> Persistent Volume -> External Storage

Regular volume can be set up relatively easily within a pod/container specification.

volumes: In the Pod spec, these specify the storage volumes available to the Pod. They specify the volumes available to the Pod. They specify the volumes type and other data that determines where and how the data is actually stored.

volumeMounts: In the Pod spec, these specify the location of the volumes within the container.

## Volume Types

* hostPath: Stores data in a specific directory on the k8s node.
* emptyDir: Stores data in a directory created by the k8s node. When the pod is deleted, the directory is deleted.
* gcePersistentDisk: Stores data on a GCE persistent disk.

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: volume-pod
  name: volume-pod
  namespace: dev
spec:
  containers:
  - name: busybox
    image: busybox
    command: ['sh', '-c', 'echo Success > /output/success.txt']
    volumeMounts:
    - name: my-volume
      mountPath: /output
  restartPolicy: Never
  volumes:
    - name: my-volume
      hostPath:
        path: /var/data
status: {}
```

Then run get pod with wide to see the status of the pod.
```
kubectl get pod volume-pod -n dev -o wide
```

Then ssh to worker node and cat the output file.

### emptyDir Example

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: shared-volume-pod
  name: shared-volume-pod
  namespace: dev
spec:
  containers:
  - name: busybox1
    image: busybox
    command: ['sh', '-c', 'while true; do echo Success! > /output/output.txt; sleep 5; done']
    volumeMounts:
    - name: my-volume
      mountPath: /output
  - name: busybox2
    image: busybox
    command: ['sh', '-c', 'while true; do cat /input/output.txt; sleep 5; done']
    volumeMounts:
    - name: my-volume
      mountPath: /input
  volumes:
    - name: my-volume
      emptyDir: {}
  restartPolicy: Never
```

To view the second container logs 
```
kubectl logs shared-volume-pod -c busybox2 -n dev
```