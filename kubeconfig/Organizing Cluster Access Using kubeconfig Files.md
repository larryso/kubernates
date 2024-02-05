# Organizing Cluster Access Using kubeconfig Files

![](../img/kubernates.webp)

Use kubeconfig files to organize information about clusters, users, namespaces, and authentication mechanisms. The kubectl command-line tool uses kubeconfig files to find the information it needs to choose a cluster and communicate with the API server of a cluster.

By default, kubectl looks for a file named config in the $HOME/.kube directory. You can specify other kubeconfig files by setting the KUBECONFIG environment variable or by setting the --kubeconfig flag.

```
kubectl --kubeconfig C:\Larry\Project\LHCDMS-LFU\K8s-connection\PROD\config get pod 
```
## The kubeconfig file components
Kubernetes components like kubelet, kube-controller-manager, or kubectl use the kubeconfig file to interact with the Kubernetes API. 

The kubeconfig file's default location for kubectl or oc is the ~/.kube directory. Instead of using the full kubeconfig name, the file is just named config. The default location of the kubeconfig file is ~/.kube/config. There are other ways to specify the kubeconfig location, such as the KUBECONFIG environment variable or the kubectl --kubeconfig parameter.

The kubeconfig file is a YAML file containing groups of clusters, users, and contexts.

```yml
apiVersion: v1
kind: Config
clusters:
- name: minikube
 cluster:
   certificate-authority: /home/hector/.minikube/ca.crt
   server: https://192.168.39.217:8443
users:
- name: minikube
 user:
   client-certificate: /home/hector/.minikube/profiles/minikube/client.crt
   client-key: /home/hector/.minikube/profiles/minikube/client.key
contexts:
- name: minikube
 context:
   cluster: minikube
   namespace: default
   user: minikube
current-context: minikube
```

The <b>clusters</b> section lists all clusters that you already connected. In this case, there is only one, named minikube. This name is arbitrary and can be any name you like. Inside the cluster are two keys:

* <b>certificate-authority</b> contains a certificate for the certificate authority (CA) that signed all internal Kubernetes certificates. This can be a file path or a Base64 string of the certificate's Privacy Enhanced Mail (PEM) format.
server is the address of the server.

* <b>server</b> is the address of the server.

The <b>users</b> section lists all users already used to connect to a cluster. In this case, there is only a single user, named minikube. This name is arbitrary, and there is no relationship with any object inside Kubernetes or OpenShift. There are some possible keys for a user:

* <b>client-certificate</b> contains a certificate for the user signed by the Kubernetes CA. This can be a file path or a Base64 string in the certificate PEM format.
* <b>client-key</b> contains the key that signed the client certificate.
* <b>token</b> contains a token for this user when there is no certificate.

The <b>contexts</b> section specifies a combination of a user and a cluster. It also defines a default namespace for this pair. The context name is arbitrary, but the user and cluster should be predefined inside the kubeconfig file. If the namespace does not exist inside Kubernetes, the commands will fail and display the default Kubernetes message for a nonexistent namespace.

## Kubeconfig file path

By default, kubectl loos for the kubeconfig file at the path $HOME/.kube/config, howevver, it is possible to specific a different kubeconfig file using the --kubeconfig flag when running kubernetes commands.

`kubectl --kubeconfig=path/to/kubeconfig get pods`

## Generate kubeconfig file





