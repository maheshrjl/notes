# storage & routing

## Storage

For databases it is better to use database as a service offering from cloud vendors.&#x20;

### Volume

* In Kubernetes volumes are tied to lifecycle of a pod
* All containers in a single pod can share the volume&#x20;

### Persistent Volume

* Created at the cluster level (It outlives the pod)
* Multiple pods can share the volume
* Seperates the storage configuration from the pod&#x20;

{% hint style="info" %}
CSI (**Container Storage Interface**) is the new way to connect to storage
{% endhint %}

## Networking

### Ingress

* Operates at OSI layer 7 (http)
* Allows routing based on hostname or url&#x20;

## StatefulSets

StatefulSet is the **workload API object used to manage stateful applications**. Manages the deployment and scaling of a set of Pods, and provides guarantees about the ordering and uniqueness of these Pods.

## CRD's & the operator pattern

Operators are software extensions to Kubernetes that make use of [custom resources](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) to manage applications and their components. _Custom resources_ are extensions of the Kubernetes API

* Allows to add 3rd party resource & controllers to kubernetes that extends the kubernetes API & CLI&#x20;
* Operators can be used to automate deployment & management of complex apps Eg: databases, monitoring tools & backups
