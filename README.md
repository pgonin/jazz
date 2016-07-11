### Docker hands-on session @ SUSE [12.07.16]

- [prerequisites](#prerequisites)
- [nginx](#nginx)
- [redis](#redis)
- [owncloud](#owncloud)
- [do it yourself](#do-it-yourslef)

## prerequisites

_In case of any errors check official docker + suse howto [here](https://docs.docker.com/engine/installation/linux/SUSE/)_

Install docker
```
$ zypper in docker
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
$ docker pull opensuse ubuntu
```


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
