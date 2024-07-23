# Kubernetes StorageClass: Concepts and Common Operations

A Kubernetes StorageClass is a Kubernetes Storage mechanism that lets you dynamiocally provision persistent volumes (PV) in a kubernetes clauster. Kubernetes administrator defines classes of storagte, and then pods can dynamically request the specific type of storage they need.

## kubernetes StorageClass concepts

Creating a StorageClass is similar to creating other kubernetes objects:
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
    name: standard
provisioner: kubernetes.io/aws-ebs
parameters:
    type: gp3
reclaimPolicy: Retain
allowVolumeExpansion: true
mountOptions:
    - debug
volumeBindingMode: Immediate
```
The name of the StorageClass Object is important because it enables requestes to that particular class. Every StorageClass has the following fields:
* Provisioner—this is the plugin used to provision the storage volume
* Parameters—indicate properties of the underlying storage system
* reclaimPolicy—indicates under what conditions the storage is released by a pod and can be reclaimed for use by other pods

These are explained in more detail below.
1. Provisioner
A StorageClass object contains a provisioner that decides which volume plugin is used to provision PersistentVolumes. Admins must specify this field.Kubernetes provides internal provisioners, which you can see listed [here](https://kubernetes.io/docs/concepts/storage/storage-classes/). Their names have a kubernetes.io prefix and they are shipped by default as part of Kubernetes.Users can specify and run external provisioners - these are independent programs that follow a Kubernetes-defined specification. The author of an external provisioner has full discretion over the code’s storage location, the provisioner’s shipping and running, and the selection of volume plugins (including Flex). To see an example, read our blog about [building a CSI driver](https://bluexp.netapp.com/blog/cvo-blg-kubernetes-csi-basics-of-csi-volumes-and-how-to-build-a-csi-driver).An example of an external provisioner is the Network File System (NFS), which is not available as an internal provisioner, but offers an external provisioner. In some cases, third-party storage vendors provide their own external provisioners.Related content: Read our guide to [Kubernetes NFS](https://bluexp.netapp.com/blog/kubernetes-nfs-two-quick-tutorials-cvo-blg)

2. Parameters
Parameters describe volumes belonging to the StorageClass. The parameters depend on the provisioner plugin and the properties of the underlying storage system.A StorageClass can have at most 512 parameters. The parameters object can have a total length of up to 256 KiB including keys and values.

3. Reclaim Policy
StorageClass can dynamically create PersistentVolumes that specify either Delete or Retain in the class’s reclaimPolicy field. If there is no specified reclaimPolicy in a StorageClass object (admins must specify it when creating the object), it is set to Delete by default:
* Setting the reclaim policy to Delete means that the storage volume is deleted when it is no longer required by the pod.
* Setting the reclaim policy to Retain means that the storage volume is retained when no longer required by the pod and can be reused by other pods. A PersistentVolume that was created manually and managed using a StorageClass retains the reclaim policy assigned to it at creation.

## Volume Binding Mode
The volumeBindingMode field of the StorageClass determines how pods bind to a storage volume. If it is not set, the Immediate mode is the default. Immediate indicates that dynamic provisioning and binding occur immediately upon the creation of a PersistentVolumeClaim. A PersistentVolumeClaim is an object that represents a request by a pod for a persistent storage volume.

A common problem is that storage backends are not globally accessible from all nodes in the cluster. In this case, PersistentVolumes might be provisioned without knowledge of the pod's scheduling requirements, resulting in unschedulable pods.Cluster administrators can address issues like this by setting the volume binding mode to WaitForFirstConsumer. This mode delays the binding and provisioning of the PersistentVolume until the creation of a pod using a matching PersistentVolumeClaim.

PersistentVolumes are selected or provisioned according to the pod's scheduling constraints. These include:
* Resource requirements
* Node selectors
* Pod affinity, anti-affinity, taints, and tolerations

## Kubernetes StorageClass Common Operations

### Using a Default StorageClass

If a cluster has a default StorageClass that meets the user’s needs, all that is necessary is to create a PersistentVolumeClaim. The default provisioner should take care of the rest—there is no need to specify the storageClassName.Below is an example of a PersistentVolumeClaim that does not use a storageClassName, and thus uses the default StorageClass.

```
apiVersion: v1
kind: PersistenceVolumeClaim
metadata:
  name: myPVC
  namespace: testns
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storeage: 10Gi
```

### Adding a Custom StorageClass
To add a custom StorageClass, it is first necessary to determine which provisioners will work in the cluster. The next step is to create a StorageClass object with custom parameters to meet the user’s needs.For many users, the easiest way to create an object is to write a yaml file and use the following command to apply it:
`kubectl create -f`
The following example shows a custom StorageClass, which provides high performance cloud storage with solid state drives (SSD) via Google Cloud Storage. The administrator could leave the default StorageClass and add this special StorageClass for workloads that require high performance.

```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata: 
  name: high-perf
provioner: kubernetes.io/gce-pd
parameters:
  type: pd-ssd
```