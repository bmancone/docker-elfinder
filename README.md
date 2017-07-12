# Docker-elfinder

[elFinder](https://github.com/Studio-42/elFinder) is an open-source file manager for web, written in JavaScript using jQuery UI.

***Note: This image is based upon [docker-baseimage-alpine](https://github.com/linuxserver/docker-baseimage-alpine) from [LinuxServer.io](https://linuxserver.io/) (which is based upon Alpine Linux and S6 overlay).***

## Usage

```
docker create --name=elfinder \
-v <path to files>=/data \
-e PGID=<gid> \
-e PUID=<uid> \
-e TZ=<timezone> \
-p 80:80 \
leadz/elfinder
```

Since it is based on alpine linux with s6 overlay, for shell access whilst the container is running do `docker exec -it elfinder /bin/bash`.

## Parameters

* `-p 80:80` - the port(s)
* `-v /data` - local path for files
* `-e PGID` for GroupID - see below for explanation
* `-e PUID` for UserID - see below for explanation
* `-e TZ` for timezone information, eg Europe/Paris

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. [LinuxServer.io's team](https://linuxserver.io/) avoid this issue by allowing you to specify the user `PUID` and group `PGID`. 

> Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1000` and `PGID=1000`. To find yours, use `id user` as below:

```
  $ id <user>
    uid=1000(leadz) gid=1000(leadz) groups=999(docker),1000(leadz)
```

## Setting up elfinder

Webui can be found at `<your-ip>:80`.

## Usage with docker-compose

Exemple :

```yaml
version: '3'

services:

  elfinder:
    image: leadz/elfinder:latest
    container_name: elfinder
    ports:
      - 80:80
    volumes:
      - <path to files>:/data
    environment:
      - TZ:<timezone>
      - PGID=<gid>
      - PUID=<uid>
```