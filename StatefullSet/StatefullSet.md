# StatefullSet

A StatefulSet is a set of pods with a unique, persistent hostname and ID. StatefulSets are designed to run stateful applications in Kubernetes with dedicated persistent storage. When pods run as part of a StatefulSet, Kubernetes keeps state data in the persistent storage volumes of the StatefulSet, even if the pods shut down.

StatefulSets are commonly used to run replicated databases with a unique persistent ID for each pod. Even if the pod is rescheduled to another machine, or moved to an entirely different data center, its identity is preserved. Persistent IDs allow you to associate specific storage volumes with pods throughout their lifecycle.

```yml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  selector:
    matchLabels:
      app: nginx
  serviceName: "nginx"
  replicas: 3
  template:
    metadata:
      labels:
        app: nginx
    spec:
      terminationGracePeriodSeconds: 10
      containers:
     —name: nginx
        image: k8s.gcr.io/nginx-slim:0.8
        ports:
       —containerPort: 80
          name: web
        volumeMounts:
       —name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
 —metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      resources:
        requests:
          storage: 1Gi
```
* The code above creates a StatefulSet called web, containing three pods running an NGINX container image.
* The specs.selector.matchLabels.app field must match the template.metadata.labels field (both are set to app: nginx). This ensures that the StatefulSet can correctly identify containers it is managing.
* The pod exposes the web port defined as port 80.
* volumeClaimTemplates provides storage using a PersistentVolumeClaim called www. This requests a storage volume that enables ReadWriteOnce access and has 1GB of storage.
* A mount path is specified by template.spec.volumeMounts and is called www. This path indicates that storage volumes should be mounted in the /usr/share/nginx/html folder within the container.

## StatefulSet vs. DaemonSet vs. Deployment
StatefulSets, DaemonSets, and Deployments are different ways to deploy pods in Kubernetes. All three of these are defined via YAML configuration. When you apply this configuration in your cluster, an object is created, which is then managed by the relevant Kubernetes controller.

### Deployments provide the following benefits:

1. Scalability: Deployments can scale up and down the number of replicas based on demand, ensuring that your application can handle increasing traffic loads.
2. Rolling Updates: Deployments can update the pods in a rolling fashion, allowing you to perform updates without any downtime.
3. Automatic Rollbacks: Deployments can automatically roll back to the previous version if an update fails, ensuring that your application remains available.

### StatefulSets provides the following benefits:
1. Ordered Pod Creation: StatefulSets ensure that each pod is created in a specific order, allowing applications to rely on the order of pod creation for initialization tasks.
2. Stable Network Identities: StatefulSets provide stable network identities for each pod, making it easy to communicate with specific pods in the set.
3. Persistent Storage: StatefulSets can manage the creation and deletion of PersistentVolumeClaims (PVCs), ensuring that each pod has a unique persistent storage.

The key differences between these three objects can be described as follows:

* StatefulSets run one or more pods with a persistent ID and persistent volumes, which is suitable for running stateful applications. unlike Deployment scaling pods, statefullSets pods are run individually.
* DaemonSets run one or more pods across the entire cluster or a certain set of nodes. This can be used to run administrative workloads such as logging and monitoring components.
* Deployments run one or more pods, allowing you to define how many replicas of the pods need to run, on which types of nodes, and which deployment strategy should be used (for example, a Rolling deployment which replaces pods with a new version one by one, to prevent downtime).

## Ref
[https://www.youtube.com/watch?v=zj6r_EEhv6s](https://www.youtube.com/watch?v=zj6r_EEhv6s)

[https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)

[https://komodor.com/learn/statefulset-basics-and-how-to-debug-a-statefulset/](https://komodor.com/learn/statefulset-basics-and-how-to-debug-a-statefulset/)

[StatefullSets Vs. Deployment](https://levelup.gitconnected.com/kubernetes-101-deployment-vs-statefulset-509058c10593)