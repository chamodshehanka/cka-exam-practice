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