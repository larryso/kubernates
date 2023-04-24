# Secrets
![](../img/kubernates.webp)

A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. Such information might otherwise be put in a Pod specification or in a container image. Using a Secret means that you don't need to include confidential data in your application code.

Because Secrets can be created independently of the Pods that use them, there is less risk of the Secret (and its data) being exposed during the workflow of creating, viewing, and editing Pods. Kubernetes, and applications that run in your cluster, can also take additional precautions with Secrets, such as avoiding writing secret data to nonvolatile storage.

Secrets are similar to ConfigMaps but are specifically intended to hold confidential data.

## Size limit 
Individual secrets are limited to 1MiB in size. This is to discourage creation of very large secrets that could exhaust the API server and kubelet memory. However, creation of many smaller secrets could also exhaust memory. You can use a resource quota to limit the number of Secrets (or other resources) in a namespace.

## Types of Secret
| Built-in Type | Usage |
| ----| ----|
|Opaque|arbitrary user-defined data|
|kubernetes.io/service-account-token|ServiceAccount token|
|kubernetes.io/dockercfg|	serialized ~/.dockercfg file|
kubernetes.io/dockerconfigjson|	serialized ~/.docker/config.json file|
kubernetes.io/basic-auth|	credentials for basic authentication|
kubernetes.io/ssh-auth|	credentials for SSH authentication|
kubernetes.io/tls|	data for a TLS client or server|
bootstrap.kubernetes.io/token|	bootstrap token data|

### Opaque secrets 


[detailed reference](https://kubernetes.io/docs/concepts/configuration/secret/)


## Using a Secret
When you create a secret, it needs to be referenced by the pod that will use it. To make a secret available for a pod:

1. Mount the secret as a file in a volume available to any number of containers in a pod.

2. Import the secret as an environment variable to a container.

3. Use kubelet, and the imagePullSecrets field.



[Refer - kubernates secret](https://kubernetes.io/docs/concepts/configuration/secret/)
