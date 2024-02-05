# Docker Command

![](../img/docker.png)

## docker pull - used to pull image from docker repository

usage: docker pull <image name>:tag

## docker run - to create a container form an image

usage: docker run [OPTIOS] IMAGE [COMMAND] [ARGS]

[more details](https://docs.docker.com/engine/reference/commandline/run/)

## docker volumne - container store data

docker volume create [OPTIONS] [VOLUME]

[more details](https://docs.docker.com/engine/reference/commandline/volume_create/)

Example: run a mysql conainer with mysql-lfu volum

docker volume create mysql-lfu

docker run -d --name=mysql-server -p 3306:3306 -v mysql-lfu:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=root larryso/lfu-db:v1

docker exec -it mysql-server mysql -u root -p
>> MYSQL  alter user 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root'
          flush privileges

The volume data in the host could be see under: \\wsl$\docker-desktop-data\version-pack-data\community\docker\volumes

## Commit Changes to Image

`docker commit [container_id] new image name`

docker commit 02747257ad63 my_ubuntu:v1.0

## Push the image to docker hub
```
docker login

## change the docker name to [dockerid]/images name:[tag]

docker tag 723390154dba larryso/my_ubuntu:v1.0

docker push larryso/my_ubuntu:v1.0
```


