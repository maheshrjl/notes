# Kubernetes

[Official kubectl  cheatsheet ](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)

## Introduction

* Kubernetes is container orchestrator making many servers act like one.
* Kubernetes usually run's on top of `docker` but can use other container runtimes like `containerd` or `crio`
* Kubernetes provides API/CLI to manage containers across servers
* Similar to linux distributions there are kubernetes distributions as well

## Kubernetes Advantages

* Cloud/Infrastructure vendors will deploy/manage kubernetes
* Wide adoption in the community
* Flexible: cover a wide range of use cases

## &#x20;Swarm Advantages

* Comes with docker, single vendor container platform
* Easiest to deploy & manage
* Can run anywhere docker does Eg: local, cloud, datacenter, ARM, windows 32-bit, 64-bit
* Secure by default (mutual tls authentication in swarm, encrypted control plane & database secrets)
* Easier to troubleshoot

## Technical Terms

**`kubectl`** - The CLI to configure & manage apps.

**`Node`** - Single server kubernetes cluster.

**`Kubelet`** - Kubernetes agent running on every node.

**`Control Plane`** - A set of container that manage the clusters(Also called master). Similar to manager in swarm. It includes `API server`, `scheduler`, `controller manager`, `etcd`, `coredns`

**`Pod`** - One or more container running together on one node. It is abstraction of a container. Containers are always in pods. Pods can be deployed directly (without containers) but it is not recommended.

**`Controllers`** - Control loops that watch the state of your cluster, then make or request changes where needed

**`Deployment`** -  Deployment provides declarative updates for Pods and ReplicaSets.

ReplicaSets&#x20;

### Replicaset

* A ReplicaSet's purpose is to maintain a stable set of replica Pods running at any given time. As such, it is often used to guarantee the availability of a specified number of identical Pods.
* A ReplicaSet ensures that a specified number of pod replicas are running at any given time.

`A ReplicaSet (RS) is a Kubernetes object used to maintain a stable set of replicated pods running within a cluster at any given time.`

### Label

Labels are key-value pairs assigned to Kubernetes Objects like Pods, Service

Example labels:

```yaml
"release" : "stable", "release" : "canary"
```

Labels can be used to:

*   find all pods that have a value associated with the key

    ```
    kubectl get pods -l key=val,key2=val2
    ```
*   merge and stream logs of the various pod that share the same label

    ```
    kubectl logs -l key=val
    ```

### Label Selectors

Unlike names and UIDs, labels do not provide uniqueness. In general, we many objects can have the same label.

Label selectors can be used to identify a set of objects. The label selector is the core grouping primitive in Kubernetes.

The API currently supports two types of selectors:

1. equality-based
   * `environment = production`
   * `tier != frontend`
2. set-based
   * environment in (production, qa)
   * tier notin (frontend, backend)
   * partition
   * !partition

### Annotations

Annotations are used to store data about the resource itself. This usually consists of machine-generated data, and can even be stored in JSON form. Eg:

* last updated&#x20;
* managed by&#x20;
* sidecar injection configuration

## Abbreviations / Short Names

<table><thead><tr><th>Long</th><th>Short</th><th data-hidden></th></tr></thead><tbody><tr><td>services</td><td>svc</td><td></td></tr><tr><td>deployment</td><td>deploy</td><td></td></tr><tr><td>nodes</td><td>no</td><td></td></tr><tr><td>pods</td><td>po</td><td></td></tr><tr><td>replicasets</td><td>rs</td><td></td></tr><tr><td>cronjobs</td><td>cj</td><td></td></tr><tr><td>events</td><td>ev</td><td></td></tr><tr><td></td><td></td><td></td></tr><tr><td><code>--all-namespaces</code></td><td>-A</td><td></td></tr></tbody></table>

