### Docker hands-on session @ SUSE [12.07.16]

- [prerequisites](#prerequisites)
- [cheat sheet](#cheat-sheet)
- [nginx](#nginx)
- [redis](#redis)
- [owncloud](#owncloud)
- [do it yourself](#do-it-yourself)

## prerequisites

_In case of any errors check official docker + suse howto [here](https://docs.docker.com/engine/installation/linux/SUSE/)_

Install docker
```
$ sudo zypper in docker
```

Run docker daemon
```
$ sudo systemctl start docker
```
The docker package creates a new group named **docker**. Users, other than **root** user, must be part of this group to interact with the Docker daemon. You can add users with this command syntax:
```
$ sudo /usr/sbin/usermod -a -G docker <username>
```

check if **IP Forwarding** is enabled:

```
sysctl net.ipv4.ip_forward 
```

if not, run
```
$ sudo sysctl -w.net.ipv4 ip_forward=1
```

Clone this repository

```
$ git clone https://github.com/Evalle/docker_hands_on.git
```

Pull some docker images

```
$ docker pull opensuse:42.1 
$ docker pull opensuse:tumbleweed
```

## cheat-sheet
### Containers Lifecycle

* [`docker create`](https://docs.docker.com/reference/commandline/create) creates a container but does not start it.
* [`docker rename`](https://docs.docker.com/engine/reference/commandline/rename/) allows the container to be renamed.
* [`docker run`](https://docs.docker.com/reference/commandline/run) creates and starts a container in one operation.
* [`docker rm`](https://docs.docker.com/reference/commandline/rm) deletes a container.
* [`docker update`](https://docs.docker.com/engine/reference/commandline/update/) updates a container's resource limits.

If you want a transient container, `docker run --rm` will remove the container after it stops.

### Starting and Stopping

* [`docker start`](https://docs.docker.com/reference/commandline/start) starts a container so it is running.
* [`docker stop`](https://docs.docker.com/reference/commandline/stop) stops a running container.
* [`docker restart`](https://docs.docker.com/reference/commandline/restart) stops and starts a container.
* [`docker pause`](https://docs.docker.com/engine/reference/commandline/pause/) pauses a running container, "freezing" it in place.
* [`docker unpause`](https://docs.docker.com/engine/reference/commandline/unpause/) will unpause a running container.
* [`docker wait`](https://docs.docker.com/reference/commandline/wait) blocks until running container stops.
* [`docker kill`](https://docs.docker.com/reference/commandline/kill) sends a SIGKILL to a running container.
* [`docker attach`](https://docs.docker.com/reference/commandline/attach) will connect to a running container.

### Info

* [`docker ps`](https://docs.docker.com/reference/commandline/ps) shows running containers.
* [`docker logs`](https://docs.docker.com/reference/commandline/logs) gets logs from container.  (You can use a custom log driver, but logs is only available for `json-file` and `journald` in 1.10)
* [`docker inspect`](https://docs.docker.com/reference/commandline/inspect) looks at all the info on a container (including IP address).
* [`docker events`](https://docs.docker.com/reference/commandline/events) gets events from container.
* [`docker port`](https://docs.docker.com/reference/commandline/port) shows public facing port of container.
* [`docker top`](https://docs.docker.com/reference/commandline/top) shows running processes in container.
* [`docker stats`](https://docs.docker.com/reference/commandline/stats) shows containers' resource usage statistics.
* [`docker diff`](https://docs.docker.com/reference/commandline/diff) shows changed files in the container's FS.

`docker ps -a` shows running and stopped containers.

`docker stats --all` shows a running list of containers.

### Import / Export

* [`docker cp`](https://docs.docker.com/reference/commandline/cp) copies files or folders between a container and the local filesystem..
* [`docker export`](https://docs.docker.com/reference/commandline/export) turns container filesystem into tarball archive stream to STDOUT.

### Executing Commands

* [`docker exec`](https://docs.docker.com/reference/commandline/exec) to execute a command in container.

To enter a running container, attach a new shell process to a running container called foo, use: `docker exec -it foo /bin/bash`.

### Images Lifecycle

* [`docker images`](https://docs.docker.com/reference/commandline/images) shows all images.
* [`docker import`](https://docs.docker.com/reference/commandline/import) creates an image from a tarball.
* [`docker build`](https://docs.docker.com/reference/commandline/build) creates image from Dockerfile.
* [`docker commit`](https://docs.docker.com/reference/commandline/commit) creates image from a container, pausing it temporarily if it is running.
* [`docker rmi`](https://docs.docker.com/reference/commandline/rmi) removes an image.
* [`docker load`](https://docs.docker.com/reference/commandline/load) loads an image from a tar archive as STDIN, including images and tags (as of 0.7).
* [`docker save`](https://docs.docker.com/reference/commandline/save) saves an image to a tar archive stream to STDOUT with all parent layers, tags & versions (as of 0.7).

### Dockerfiles

* [FROM](https://docs.docker.com/reference/builder/#from) Sets the Base Image for subsequent instructions.
* [MAINTAINER](https://docs.docker.com/reference/builder/#maintainer) Set the Author field of the generated images..
* [RUN](https://docs.docker.com/reference/builder/#run) execute any commands in a new layer on top of the current image and commit the results.
* [CMD](https://docs.docker.com/reference/builder/#cmd) provide defaults for an executing container.
* [EXPOSE](https://docs.docker.com/reference/builder/#expose) informs Docker that the container listens on the specified network ports at runtime.  NOTE: does not actually make ports accessible.
* [ENV](https://docs.docker.com/reference/builder/#env) sets environment variable.
* [ADD](https://docs.docker.com/reference/builder/#add) copies new files, directories or remote file to container.  Invalidates caches. Avoid `ADD` and use `COPY` instead.
* [COPY](https://docs.docker.com/reference/builder/#copy) copies new files or directories to container.
* [ENTRYPOINT](https://docs.docker.com/reference/builder/#entrypoint) configures a container that will run as an executable.
* [VOLUME](https://docs.docker.com/reference/builder/#volume) creates a mount point for externally mounted volumes or other containers.
* [USER](https://docs.docker.com/reference/builder/#user) sets the user name for following RUN / CMD / ENTRYPOINT commands.
* [WORKDIR](https://docs.docker.com/reference/builder/#workdir) sets the working directory.
* [ARG](https://docs.docker.com/engine/reference/builder/#arg) defines a build-time variable.
* [ONBUILD](https://docs.docker.com/reference/builder/#onbuild) adds a trigger instruction when the image is used as the base for another build.
* [STOPSIGNAL](https://docs.docker.com/engine/reference/builder/#stopsignal) sets the system call signal that will be sent to the container to exit.
* [LABEL](https://docs.docker.com/engine/userguide/labels-custom-metadata/) apply key/value metadata to your images, containers, or daemons.

## nginx

openSUSE Dockerfile for nginx

To build:

    # docker build -t <username>/nginx .

To run:

    # docker run -d -p 80:80 <username>/nginx

To test:

    # curl http://localhost

## redis

OpenSUSE Dockerfile for Redis - open source, advanced key-value cache and store.  

***you need to install redis on your machine to test it ( zypper in redis )***

To build:

    # sudo docker build -t <username>/redis .

To run:

    # sudo docker run -d -p 6379 --name redis <username>/redis

To test:

  check ports:

      # sudo docker port redis 6379
    
    and via redis-cli
      
        # redis-cli -h 127.0.0.1 -p <port>
        redis 127.0.0.1:<port>ping
            PONG

## owncloud

Let's do some different stuff. This repo contains a recipe for making Docker container for OwnCloud on openSUSE Leap 42.1.

Perform the build

    # docker build -t <yourname>/owncloud .

Check the image out.

    # docker images

Run it:

    # docker run -d -p 80:80 <yourname>/owncloud

You should now be able to view the OwnCloud setup page by going to https://localhost/owncloud

## do-it-yourself
Now it's time to get your hands dirty! Do it yourself! 
