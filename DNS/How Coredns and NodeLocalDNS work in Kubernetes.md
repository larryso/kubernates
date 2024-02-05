# How CoreDNS and NodeLocalDNS Work in a Kubernetes Cluster

DNS resolution is the process of mapping a domain name to an IP address. In a Kubernetes cluster, this is used to resolve both service records and external DNS records. A service record maps the name of a service to its IP address, while an external DNS record maps a domain name to an IP address outside the cluster.

## CoreDNS

CoreDNS is the default DNS server in Kubernetes and is responsible for handling the resolution of service records. CoreDNS runs as a pod in the cluster and listens for DNS requests from other pods. When a pod makes a request for a service record, CoreDNS uses the Kubernetes API server to look up the IP address associated with the requested service. The API server then returns the IP address to CoreDNS, which in turn returns it to the requesting pod.

## NodeLocalDNS

NodeLocalDNS is a new feature in Kubernetes that enables you to run a local instance of the DNS server on each node in the cluster. NodeLocalDNS is used to resolve external DNS records, such as the IP addresses of external services. This can improve performance compared to relying on a centralized CoreDNS instance to resolve external DNS records, as the local NodeLocalDNS instance can cache DNS results and avoid network latency.

In RKE2, NodeLocalDNS runs as a DaemonSet on each node in the cluster. The NodeLocalDNS pod runs a DNS server that listens for DNS requests from pods on the node. When a pod makes a DNS request, the request is first sent to the local NodeLocalDNS instance which binds to IP address 169.254.20.10. If the requested record is present in the local cache, NodeLocalDNS returns the cached result to the pod. If the record is not present in the cache, NodeLocalDNS sends the request to the CoreDNS pods, retrieves the corresponding IP address, caches the result, and returns it to the pod.

## Example: Service Record Resolution

Let’s take a look at how a DNS request goes from a pod to CoreDNS for a service record. Suppose you have a pod running an application that needs to access a service called “nginx-svc”. The application makes a DNS request for “nginx-svc”, which is received by CoreDNS. CoreDNS then uses the Kubernetes API server to look up the IP address associated with the “nginx-svc” service. The API server returns the IP address, which CoreDNS returns to the requesting pod. The pod can now use the IP address to connect to the “nginx-svc” service.

## Example: External DNS Record Resolution
In this example, let’s assume that a pod needs to resolve the hostname “www.google.com”. The pod sends a DNS request to its local DNS resolver, which is CoreDNS in this case. CoreDNS receives the request and checks its configuration to determine the next steps.

If CoreDNS has a configured stub domain that matches “www.google.com”, it can return the corresponding IP address without having to perform any external lookups. If not, CoreDNS checks if NodeLocal DNS cache is enabled in the cluster.

If NodeLocal DNS cache is enabled, CoreDNS sends the request to the NodeLocal DNS cache. NodeLocal DNS cache maintains a local cache of frequently used external DNS records on each node. If the hostname “www.google.com” is present in the cache, NodeLocal DNS cache returns the corresponding IP address to CoreDNS.

If the hostname is not present in the cache, NodeLocal DNS cache sends the request to the external DNS servers, retrieves the corresponding IP address, caches the result, and returns it to CoreDNS. CoreDNS then returns the IP address to the pod that initiated the request.

In this way, CoreDNS and NodeLocal DNS cache work together to resolve external DNS records, providing fast and efficient resolution while reducing the load on the external DNS servers.

Finally, CoreDNS is configured to use the upstream DNS servers specified in the cluster configuration. CoreDNS will send the request to the upstream DNS servers. The upstream DNS servers will then perform the DNS lookup and return the corresponding IP address to CoreDNS. CoreDNS then returns the IP address to the pod that initiated the request. In this way, CoreDNS and NodeLocal DNS cache work together to resolve external DNS records, providing fast and efficient resolution while reducing the load on the external DNS servers.

## Conclusion

CoreDNS is a critical component of the Kubernetes networking stack, responsible for providing DNS resolution to pods and services within a cluster. By integrating NodeLocal DNS cache, CoreDNS offers improved performance and reduced load on external DNS servers, providing a fast and reliable DNS resolution experience for Kubernetes applications. Whether you are deploying a small application or a complex microservice architecture, CoreDNS and NodeLocal DNS cache provide a flexible and robust DNS infrastructure to meet your needs.

## 
![https://support.tools/post/kubernetes-coredns/](https://support.tools/post/kubernetes-coredns/)