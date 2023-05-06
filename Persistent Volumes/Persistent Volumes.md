# Persistent Volumes

## Volumes

Docker has a concept of volumes, though it is somewhat looser and less managed. A Docker volume is a directory on disk or in another container. Docker provides volume drivers, but the functionality is somewhat limited.

Docker container is a process, by default container store data on file system like any other process, container file system is temporary and not persistent after container restart.

Let's check this out by a demo:

```
# run postgres
docker run -d --rm -e POSTGRES_DB=postgresdb -e POSTGRES_USER=admin -e POSTGRES_PASSWORD=admin123 postgres:10.4

# enter the container
docker exec -it <contaner id> bash

# login to postgres
psql --username=admin postgresdb

```


## Refrence 

[https://kubernetes.io/docs/concepts/storage/persistent-volumes/](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)

[https://www.youtube.com/watch?v=ZxC6FwEc9WQ](https://www.youtube.com/watch?v=ZxC6FwEc9WQ)