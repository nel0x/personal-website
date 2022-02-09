+++
title = "Dockerize Homelab"
date = "2022-02-08T16:00:00+01:00"
tags  = ['docker', 'homelab', 'tech']
+++

Though I love docker and containerization, most of my infrastructure is still hosted on Virtual Machines via Proxmox.
This has some obvious drawbacks like slower (re-)starting times, the use of more ressources due to the virtualized kernel & emulated hardware and harder automation of the deployment process.

That's why I finally got myself to migrating my infrastructure over as well as discovering some new sysyems while exploring docker.

I will document the whole procedure with all it's obstacles on this page, while starting with the basic and easy ones to warm up and then coming to more complex subjects, like storage sharing with Nextcloud and Syncthing.

![](/posts/docker.png)

## Table of contents:
- [Starting out with the basics](#basics)
- [Reverse Proxy & Let's Encrypt certs](#reverse-proxy )
- [Dashboard: DashMachine](#dashboard)
- [Documentation: Bookstack, Docusaurus, etc.]()
- [UniFi Controller & other trivia (server-stuff.md)]()
- [Media Server: Jellyfin & Sabnzbd]()
- [Storage: Nextcloud & Syncthing]()
- [Security: SingleSignOn, Authelia, Teleport]()

# Basics
First and foremost I will be using Ubuntu 20.04 LTS Server as my host system. But in most cases - as we'll be working inside the Docker environment - this should not matter. Also note that Docker commands have always to be executed with root privileges.
A bunch of available images can be found at https://hub.docker.com.

Helpfull through the setup will also be:
- https://dbtechreviews.com/blog/
- https://www.the-digital-life.com/
- https://docs.technotim.live/

1. Install Docker and Docker-compose
```
$ sudo apt install docker.io docker-compose
```
  
2. Portainer management UI Deployment
[official docs](https://docs.portainer.io/v/ce-2.11/start/install/server/docker/linux)

```
# docker volume create portainer_data
```

```
# docker run -d -p 8000:8000 -p 9443:9443 --name portainer \
    --restart=always \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -v portainer_data:/data \
    portainer/portainer-ce:2.11.1
```
-> **Go to** `https://<IP>:9443`

# Reverse Proxy
## NGNIX Proxy Manager
Eventually later on I will go with Caddy, or Traeffic or HAProxy. But for now let's start with NGNIX Proxy Manager. [official docs](https://nginxproxymanager.com/guide/#quick-setup)
```
# mkdir /mnt/reverse-proxy/ && cd /mnt/reverse-proxy/ && vim docker-compose.yml && docker-compose up -d
```
```
version: '3'
services:
  app:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80'
      - '81:81'
      - '443:443'
    volumes:
      - ./data:/data
      - ./letsencrypt:/etc/letsencrypt
```
-> **Go to** `http://<IP>:81` and create Proxy Hosts with config and SSL-certificates.

# Dashboard
Personally, I found DashMachine by far best suited for my use case but there are dozens of options available e.g. on https://github.com/awesome-selfhosted/awesome-selfhosted#personal-dashboards

https://dbtechreviews.com/2020/03/how-to-install-dashmachine-dashboard-on-docker-and-omv5/
```
# mkdir /mnt/dashmachine/ && cd /mnt/dashmachine/ && vim docker-compose.yml && docker-compose up -d
```
```
---
version: "3"
services:
  dashmachine:
    image: rmountjoy/dashmachine:latest
    container_name: dashmachine
    volumes:
      - ./config:/dashmachine/dashmachine/user_data
    ports:
      - 5000:5000
    restart: unless-stopped
```
-> **Go to** `http://<IP>:5000`
For the config.ini [this is my setup](/unlisted/dashmachine-config).

Looks amazing, right! :)

![](/posts/dashmachine.png)

