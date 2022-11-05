# pods

### Create Pods

Create ngnix web-server pod

```bash
kubectl run mhs-nginx --image nginx
```

### List Pods

```
This will show pods only in the default namespace
```

```
kubectl get pods
```

List pods with custom namespace

```
kubectl get pods --namespace=kube-system
```

List pods with IP address

```
kubectl get pod -o wide
```

List pods& refresh continously

```
kubectl get pods -o wide -w
```

Cluster info

```
kubectl cluster-info
```

#### Describe pods

```
kubectl describe pod mhs-nginx
```

#### Delete pods

```
kubectl delete pod mhs-nginx
```

#### SSH into a pod

```
kubectl exec -it hello -- /bin/sh
```
