# Dockerfile for building your container image
![](../img/docker.png)
A Dockerfile is a script that contains instructions for building a customized docker image. Each instruction in Dockerfile creates a new layer in the image, and the final image is composed of all the layers stacked on top of each other.

Below is the simple structure of Dockerfile:

```
FROM node:14-alpine3.16

WORKDIR /app

COPY . .

RUN npm install

CMD ["npm", "start"]
```
[more dockerfile instruction ->](https://www.cnblogs.com/52py/p/15152083.html)

## Docker CMD, ENTRYPOINT what's the Difference and how to Choose
The <b>ENTRYPOINT</b> specific a command that will always be executed when the container starts. it cannot be overriden when docker containers run with CLI parameters

The <b>CMD</b> sets default parameters can be overriden from the docker CLI when running the container

The <b>RUN</b> Mainly used to build images and install applications and packages, RUN builds a new layer over the existing image by committing the results.

Refer: [https://devtron.ai/blog/cmd-and-entrypoint-differences/](https://devtron.ai/blog/cmd-and-entrypoint-differences/)

## Build Docker Image:

Build a image use current folder Dockerfile and tag it with larryso/ubunt:v1, (. stands for 当前路径作为构建上下文)
`docker build -t larryso/ubunt:v1 . `

Build with a specified dockerfile
`docker build -f /path/to/dockerfile . `

## Tag and push the image
```
docker tag test:v1 username/test:v1
docker push username/test:v1
```






## Multistage build feature
Multistage builds feature in Dockerfiles enables you to create smaller container images with better caching and smaller security footprint.

### Inheriting from a stage
Multistage builds added a couple of new syntax concepts. First of all, you can name a stage that starts with a FROM command with AS stagename and use --from=stagename option in a COPY command to copy files from that stage. In fact, FROM command and --from flag have much more in common and it is not accidental that they are named the same. They both take the same argument, resolve it and then either start a new stage from that point or use it as a source for file copy.

[Please go there for more ->](https://www.docker.com/blog/advanced-dockerfiles-faster-builds-and-smaller-images-using-buildkit-and-multistage-builds/)

## scratch image
在 DockerFile 中必须指定基础镜像，FROM 指令就是用于指定基础镜像，因此一个 Dockerfile 中 FROM 是必备的指令，并且必须是第一条指令。

Docker 还存在一个特殊的镜像，名为 scratch。这个镜像是虚拟的概念，并不实际存在，它表示一个空白的镜像。在 Dockerfile 中以 scratch 为基础镜像 (FROM scratch)，意味着不以任何镜像为基础，接下来所写的指令将作为镜像第一层开始存在。

对于 Linux 下静态编译的程序来说，并不需要有操作系统提供运行时支持，所需的一切库都已经在可执行文件里了，因此直接 FROM scratch 会让镜像体积更加小巧。使用 Go 语言开发的应用很多会使用这种方式来制作镜像，这也是为什么有人认为 Go 是特别适合容器微服务架构的语言的原因之一。
