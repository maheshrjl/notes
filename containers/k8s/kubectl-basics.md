# kubectl basics

## Pods

### Create, manage &  list pods

<details>

<summary>3 ways to create pods with kubectl</summary>

* `kubectl run` (run single pod per command)
* `kubectl create` (create resources via CLI or YAML)
* `kubectl apply` (create/update resources via YAML)

</details>

#### Create pod

```bash
kubectl run mhs-nginx --image nginx
```

#### List Pods

```
kubectl get pods
```

{% hint style="warning" %}
This will show pods only in the default namespace
{% endhint %}

List pods with custom namespace

```
kubectl get pods --namespace=kube-system
```

List pods with IP address

```
kubectl get pod -o wide
```

List pods & refresh continously

```
kubectl get pods -o wide -w
```

#### Describe Pods

```
kubectl describe pod mhs-nginx
```

#### Delete Pods

```
kubectl delete pod mhs-nginx
```

#### SSH into a pod

```
kubectl exec -it hello -- /bin/sh
```

### Nodes

{% hint style="info" %}
A pod always runs on a node
{% endhint %}

#### Get Nodes

```
kubectl get nodes
```



## Namespaces

In Kubernetes, namespaces provides a mechanism for isolating groups of resources within a single cluster.

Kubernetes starts with four initial namespaces:

* `default` The default namespace for objects with no other namespace
* `kube-system` The namespace for objects created by the Kubernetes system
* `kube-public` This namespace is created automatically and is readable by all users (including those not authenticated). This namespace is mostly reserved for cluster usage, in case that some resources should be visible and readable publicly throughout the whole cluster. The public aspect of this namespace is only a convention, not a requirement.
* `kube-node-lease` This namespace holds Lease objects associated with each node. Node leases allow the kubelet to send heartbeats so that the control plane can detect node failure.

#### List all namespaces in cluster

```
kubectl get namespaces
```

#### Start a pod with namespace

```
kubectl run nginx --image=nginx --namespace=<insert-namespace-name-here>
```

## Deployments

Deployment managed by the `Kubernetes Deployment controller` tells Kubernetes how to create or modify instances of the pods that hold a containerized application. A Deployment runs multiple replicas of your application and automatically replaces any instances that fail or become unresponsive.

#### Create/scale deployment

```
kubectl create deployment nginx-deploy --image=nginx
```

#### Get deployment details

```
kubectl describe deployment nginx-deploy
```

#### Scale deployment

```
kubectl scale deployment nginx-deploy --replicas=5
```

#### Update deployment with new image

```
kubectl set image deployment k8s-hello k8s-hello=nginx:latest
```

#### Check rollout status

```
kubectl  rollout status deploy k8s-hello
```

#### Remove deployment

```
kubectl delete deployment nginx-deploy
```

## k8s Cleanup&#x20;

#### Delete kubernetes resources the default namespace

```
kubectl delete all --all
```

{% hint style="info" %}
This also deletes the default kubernetes(ClusterIP) service but it is re-created automatically
{% endhint %}

### kubectl output formatting

| Output format | Description                                                                                              |
| ------------- | -------------------------------------------------------------------------------------------------------- |
| `-o=json`     | Output a JSON formatted API object                                                                       |
| `-o=yaml`     | Output a YAML formatted API object                                                                       |
| `-o=name`     | Print only the resource name and nothing else                                                            |
| `-o=wide`     | Output in the plain-text format with any additional information, and for pods, the node name is included |
