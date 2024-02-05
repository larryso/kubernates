# Setup Local Kubernates Environment
![](./img/kubernates.webp)
1. install Chocolatey
Chocolatey是一款Windows的包管理工具，它可以像Linux下的包管理工具apt或yum一样，帮助我们方便地安装和管理Windows下的软件和工具。
下面是Chocolatey的安装方式：
打开PowerShell窗口，选择以管理员身份运行PowerShell

在命令窗口中执行以下命令，来安装Chocolatey
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

```
安装完成后，执行以下命令验证Chocolatey是否安装成功

`choco -v`
可以看到命令窗口输出了Chocolatey的版本号

最后，可以使用以下命令更新Chocolatey

`choco upgrade chocolatey`

需要注意的是，使用Chocolatey安装软件时，需要以管理员身份运行PowerShell。此外，Chocolatey安装的软件默认会被安装在C:\ProgramData\chocolatey\lib目录下。

2. Install kind

 kind 是一个使用 Docker 容器“节点”运行 Kubernetes 集群的工具。使用 kind 工具搭建的 Kubernetes 集群主要用于测试，但也可以用于本地开发和调试。

`choco install kind`

## Create a cluster by using kind

```
kind create cluster --name rbac --image kindest/node:v1.20.2
```