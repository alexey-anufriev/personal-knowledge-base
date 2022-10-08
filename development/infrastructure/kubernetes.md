# Kubernetes

## Pods and Nodes

To find all the pods and corresponding nodes run:

```bash
kubectl get pod \
  -o=custom-columns=NAME:.metadata.name,STATUS:.status.phase,NODE:.spec.nodeName \
  --all-namespaces
```

## PV / PVC

Cleanup PV ownership:

```
kubectl patch pv pg-volume -p '{"spec":{"claimRef": null}}'
```

## Random Secret

```
head /dev/urandom | tr -dc A-Za-z0-9 | head -c 16 | xargs -I {} kubectl create secret generic redis-password --from-literal=password={}
```
