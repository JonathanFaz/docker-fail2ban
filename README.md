# SuperITMan/fail2ban

## About

Available on Docker Hub: [superitman/fail2ban](https://hub.docker.com/r/superitman/fail2ban/)

A simple container with Fail2Ban installed inside to use in docker environment.

This is compatible with docker web hosts. For this, you should use action **docker-iptables-multiport** which works as iptables-multiport

The current installed version is fail2ban-0.8.13-1 (latest one available on Debian Jessie repository)

## Usage
##### To run it:

* Docker >= 1.2: ([See GitHub issue](https://github.com/moby/moby/issues/33605#issuecomment-307361421))
```
$ docker run -d -it \
-v /var/log:/var/log \
--name fail2ban \
--net host \
--cap-add=NET_ADMIN \
--cap-add=NET_RAW \
superitman/fail2ban:latest
```

* (Docker < 1.2):
```
$ docker run -d -it \
-v /var/log:/var/log \
--name fail2ban \
--net host \
--privileged \
superitman/fail2ban:latest
```
 
##### If you want to use your own jail.local and your own filters:
```
$ docker run -d -it \
-v /path/to/filter.d/custom_filter.conf:/etc/fail2ban/filter.d/custom_filter.conf \
-v /path/to/jail.local:/etc/fail2ban/jail.local \
-v /var/log:/var/log \
--name fail2ban \
--net host \
--cap-add=NET_ADMIN \
--cap-add=NET_RAW \
superitman/fail2ban:latest
```

If you want to sync fail2ban docker timezone with your host, add this argument
```
-v /etc/timezone:/etc/timezone.host:ro
```

### Lots of filters and actions

If you have a lot of custom rules, build your own docker image
```
$ git clone https://github.com/superitman/docker-fail2ban.git
$ cd docker-fail2ban
```
Copy your filters in filter.d folder, your actions in action.d and your jail.local in the current folder.

Then build your image
```
$ docker build -t your-custom-fail2ban .
```

When your image is ready, run it:
```
$ docker run -d -it \
-v /var/log:/var/log \
--name fail2ban \
--net host \
--privileged \
your-custom-fail2ban
```

## To-Do

* Should edit jail.conf to use docker-iptables-multiport action
