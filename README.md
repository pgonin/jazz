### Docker hands-on session @ SUSE

0. prerequisities
    
    
```
$ zypper in docker
```
```
$ sudo usermod -Ga .... 
```

check that ipv4 forwarding is working via:

if not, run
```
$ sudo sysctl -w.net.ipv4 ip_forward=1
$ sudo systemctl restart (or reboot)
```

check result again

```
$ git clone <this repo>
```

1. nginx
2. redis
3. owncloud
4. docker b-day application


**nginx**

openSUSE Dockerfile for nginx

To build:

    # docker build -t <username>/nginx .

To run:

    # docker run -d -p 80:80 <username>/nginx

To test:

    # curl http://localhost

**redis**

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

**owncloud**

**docker b-day application**
