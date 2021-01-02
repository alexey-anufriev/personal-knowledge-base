# Kubernetes

## Pods and Nodes

To find all the pods and corresponding nodes run:

```bash
kubectl get pod -o=custom-columns=NAME:.metadata.name,STATUS:.status.phase,NODE:.spec.nodeName --all-namespaces
```

