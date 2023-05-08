# Persistent Volumes

## Volumes

Docker has a concept of volumes (Docker has two options for containers to store files on the host machine - volumes, and bind mounts), Volumes are a mechanism for storing data outside containers. All volumes are managed by Docker and stored in a dedicated directory on your host, usually /var/lib/docker/volumes for Linux systems.

Volumes are mounted to filesystem paths in your containers. When containers write to a path beneath a volume mount point, the changes will be applied to the volume instead of the container’s writable image layer. The written data will still be available if the container stops – as the volume’s stored separately on your host, it can be remounted to another container or accessed directly using manual tools.

![](../img/types-of-mounts.png)

* Volumes are stored in a part of the host filesystem which is managed by Docker (/var/lib/docker/volumes/ on Linux). Non-Docker processes should not modify this part of the filesystem. Volumes are the best way to persist data in Docker.

* Bind mounts may be stored anywhere on the host system. They may even be important system files or directories. Non-Docker processes on the Docker host or a Docker container can modify them at any time.

* tmpfs mounts are stored in the host system’s memory only, and are never written to the host system’s filesystem.

Let's check this out by a demo:

```
# run postgres
docker run -d --rm -e POSTGRES_DB=postgresdb -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin123 postgres:10.4

# enter the container
docker exec -it <contaner id> bash

# login to postgres
psql --username=admin postgresdb

#create a table
CREATE TABLE COMPANY(
   ID INT PRIMARY KEY     NOT NULL,
   NAME           TEXT    NOT NULL,
   AGE            INT     NOT NULL,
   ADDRESS        CHAR(50),
   SALARY         REAL
);

#show table
\dt

# quit 
\q
```
Restarting the above container and going back in you will notice \dt commands returning no tables. Since data is lost.
```
#exit container and rm container 
docker rm -f <contaner id>
```
### Persist data Docker
```
docker volume create postges

#docker run -v /path/on/host:/path/inside/container image -- Then all your data will persist in /path/on/host; 
docker run -d --rm -v postges:/var/lib/postgresql/data -e POSTGRES_DB=postgresdb -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin123 postgres:10.4

# run the same tests as above and notice
```

## Kubernates Persistence Volume


Same can be demonstrated using Kubernetes

```
cd .\kubernetes\persistentvolume\

kubectl create ns postgres
kubectl apply -n postgres -f ./postgres-no-pv.yaml
kubectl -n postgres get pods 
kubectl -n postgres exec -it postgres-0 bash

# run the same above mentioned commands to create and list the database table

kubectl delete po -n postgres postgres-0

# exec back in and confirm table does not exist.
```

## Refrence 

[https://kubernetes.io/docs/concepts/storage/persistent-volumes/](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

[https://www.youtube.com/watch?v=ZxC6FwEc9WQ](https://www.youtube.com/watch?v=ZxC6FwEc9WQ)

[Docker Volume](https://spacelift.io/blog/docker-volumes)

[Docker Storage](https://docs.docker.com/storage/)

[git-kubernetes](https://github.com/marcel-dempers/docker-development-youtube-series)