# Docker Networking
![](../img/docker-network.png)
[https://docs.docker.com/network/](https://docs.docker.com/network/)
Docker includes a networking system for managing communications between containers, your Docker host, and the outside world. Several different network types are supported, facilitating a variety of common use cases.

## Docker Network Types
* bridge
* host
* overlay
* IPvLAN
* macvlan

### 1. bridge
Bridge networks create a software-based bridge between your host and the container. Containers connected to the network can communicate with each other, but they’re isolated from those outside the network.

Each container in the network is assigned its own IP address. Because the network’s bridged to your host, containers are also able to communicate on your LAN and the internet. They will not appear as physical devices on your LAN, however.

* 每个容器之间网络协议栈独立，Docker会创建一个docker0虚拟网桥，并在主机上自动为其分配一个IP地址。
* 新创建的容器会与docker0网桥相连，并通过veth pair设备共享IP地址和其他网络信息。
* 这个模式下，容器之间可以进行通信，并且容器可以共享宿主机的网络配置，如IP地址和端口范围
