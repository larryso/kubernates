# Organizing Cluster Access Using kubeconfig Files

![](../img/kubernates.webp)

Use kubeconfig files to organize information about clusters, users, namespaces, and authentication mechanisms. The kubectl command-line tool uses kubeconfig files to find the information it needs to choose a cluster and communicate with the API server of a cluster.

By default, kubectl looks for a file named config in the $HOME/.kube directory. You can specify other kubeconfig files by setting the KUBECONFIG environment variable or by setting the --kubeconfig flag.

```
kubectl --kubeconfig C:\Larry\Project\LHCDMS-LFU\K8s-connection\PROD\config get pod 
```
