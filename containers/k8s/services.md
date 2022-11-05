# services

Service is abstract way to expose an application running on a set of Pods as a network service.

### ClusterIP

ClusterIP is a type of service which is only accessible within a Kubernetes cluster, or the internal ("virtual") IP of components within a Kubernetes cluster.

* ClusterIP is the default and most common service type.
* Kubernetes will assign a cluster-internal IP address to ClusterIP service. This makes the service only reachable within the cluster.
* You cannot make requests to service (pods) from outside the cluster.
* You can optionally set cluster IP in the service definition file.

#### Create a [ClusterIp](services.md#clusterip)

Expose a internal port 80 to the external port 8080

```
kubectl expose deployment nginx-deploy --port=8080 --target-port=80
```

{% hint style="warning" %}
ClusterIP service will allow connecting from inside the kubernetes cluster. Used when we want to block connections from outside the kubernetes cluster but want the cluster itself to have access.
{% endhint %}

### NodePort

NodePort builds on top of the ClusterIP Service and provides a way to expose a group of Pods to the outside world.

* A ClusterIP Service, to which the NodePort Service routes, is automatically created.
* It exposes the service outside of the cluster by adding a cluster-wide port on top of ClusterIP. NodePort exposes the service on each Node’s IP at a static port (the NodePort). Each node proxies that port into your Service. So, external traffic has access to fixed port on each Node. It means any request to your cluster on that port gets forwarded to the service.
* You can contact the NodePort Service, from outside the cluster, by requesting :.
* Node port must be in the range of 30000–32767. Manually allocating a port to the service is optional. If it is undefined, Kubernetes will automatically assign one.

#### Create a [NodePort](services.md#nodeport)

```
kubectl expose deployment k8s-hello --type=NodePort --port=3000
```

{% hint style="danger" %}
NodePort service will allow connecting from outside the kubernetes cluster
{% endhint %}

### LoadBalancer

* LoadBalancer service is an extension of NodePort service. NodePort and ClusterIP Services, to which the external load balancer routes, are automatically created.
* It integrates NodePort with cloud-based load balancers & exposes the Service externally using a cloud provider’s load balancer.
* The cloud providers assigns the external IP to the LoadBalancer automatically
* In local k8s cluster LoadBalancer behaves similar to NodePort

#### Create a [LoadBalancer](services.md#loadbalancer)

```
kubectl expose deployment k8s-hello --type=LoadBalancer --port=3000
```

### ExternalName

* Adds CNAME records to CoreDNS&#x20;
* You specify these Services with the \`spec.externalName\` parameter.
* It maps the Service to the contents of the externalName field (e.g. foo.bar.example.com), by returning a CNAME record with its value.
* No proxying of any kind is established.

#### Use Cases <a href="#c08f" id="c08f"></a>

* This is commonly used to create a service within Kubernetes to represent an external datastore like a database that runs externally to Kubernetes.

### Service DNS&#x20;

*   Services in kubernetes have FQDN to communicate across namespaces. This DNS can resolve inside the kubernetes cluster only.

    Format: `<hostname>.<namespace>.svc.cluster.local`
