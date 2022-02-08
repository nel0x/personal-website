+++
title = "Dockerize Homelab"
date = "2021-12-08T17:00:00+01:00"
tags  = ['docker', 'homelab', 'tech']
+++

Thought I love docker and containerization, most of my infrastructure is still hosted on Virtual Machines via Proxmox.
This has some obvious drawbacks like slower (re-)starting times, the use of more ressources due to the virtualized kernel & emulated hardware and harder automatisation of the deployment process.

That's why I finally got myself to migrating my infrastructure over as well as discovering some new sysyems while exploring docker.

I will document the whole procedure with all it's obstacles on this page, while starting with the basic and easy ones to warm up and then coming to more complex subjects, like storage sharing with Nextcloud and Syncthing.

## Table of contents:
- [Reverse Proxy & Let's Encrypt certs]()
- [Webdav & simple site]()
- [Dashboard: Heimdall/Homer, etc.]()
- [Documentation: Bookstack, Docusaurus, etc.]()
- [UniFi Controller & other trivia (server-stuff.md)]()
- [Media Server: Jellyfin & Sabnzbd]()
- [Storage: Nextcloud & Syncthing]()
- [Virtualize router with OPNSense]()
- [Security: SingleSignOn, Authelia, Teleport]()
