# HOW TO RUN CRON JOBS INSIDE DOCKER

Docker is build for running one process per container. That doesn't mean that you can't run more than one process per container, it's just that docker only cares about the first process of an image. This process is what you specify using the ENTRYPOINT in your Dockerfile and it's the process that docker starts when you run an image.

Should that process terminate, docker will stop the container and if that process writes to stdout or stderr, docker will make that information available via docker logs [container] and if you run the image attached your keyboard input will be forwarded to your ENTRYPOINTs stdin.

If you want to run multiple processes, you can start additional ones from your ENTRYPOINT. Docker doesn't care about those child processes. Should one of them terminate, your container will continue to run - if they die, they die. And if they write stuff to stdout or stderr, docker doesn't know and doesn't care.

## CRON IS A DAEMON
cron is usually run as a daemon so on startup cron forks, which means it creates a running copy of itself and then terminates the original process.

Which is normally what you want, but when running inside a container docker will stop your container when your main process exits.

## RUNNING CRON INSIDE A DOCKER IMAGE
The way solve that problem is to instruct cron to start in foreground mode, so that it doesn't fork by using:

```
cron -f
```
By running cron in foreground mode you can use it as an ENTRYPOINT in your Dockerfile and it will execute the jobs you've specified in your crontab file.

## LOGGING OF CRON JOBS
There is one problem left, which is that the output of those jobs don't show up in the docker logs, because docker only tracks the stdout/stderr of the first process.

A simple solution for this problem is just redirecting the stdout/stderr of the child processes to the main process (which will always have PID 1), e.g.:

```
*/1 * * * * root /app1/test.sh > /proc/1/fd/1 2>/proc/1/fd/2
```


When running cron as your ENTRYPOINT in a container, make sure you start cron in foreground mode (cron -f) and consider redirecting your job's stdout and stderr to crons (eg. */1 * * * * root /app1/test.sh > /proc/1/fd/1 2>/proc/1/fd/2). 

```Dockerfile
FROM ubuntu

# install cron
RUN apt-get update && apt-get install cron -y -qq

# create two test applications that we will launch using cron
RUN mkdir /app1 && echo 'echo `date +"%H:%M:%S"` - This is sample application 1!' > /app1/test.sh && chmod +x /app1/test.sh
RUN mkdir /app2 && echo 'echo `date +"%H:%M:%S"` - This is sample application 2!' > /app2/test.sh && chmod +x /app2/test.sh

# register cron jobs to start the applications and redirects their stdout/stderr
# to the stdout/stderr of the entry process by adding lines to /etc/crontab
RUN echo "*/1 * * * * root /app1/test.sh > /proc/1/fd/1 2>/proc/1/fd/2" >> /etc/crontab
RUN echo "*/2 * * * * root /app2/test.sh > /proc/1/fd/1 2>/proc/1/fd/2" >> /etc/crontab

# start cron in foreground (don't fork)
ENTRYPOINT [ "cron", "-f" ]
```

Build and run:

```
docker build . -t cron
docker run cron
```