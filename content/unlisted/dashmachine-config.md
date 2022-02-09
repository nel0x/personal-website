+++
title = "DashMachine Config"
date = "2025-02-08T16:00:00+01:00"
+++
```
[Settings]
theme = dark
accent = amber
background = random
roles = admin
home_access_groups = admin_only
settings_access_groups = admin_only
custom_app_title = DashMachine
sidebar_default = open
tags_expanded = true
tags = {"name": "Home", "icon": "home", "sort_pos": 1},{"name": "Work", "icon": "folder", "sort_pos": 2},{"name": "Media", "icon": "movie", "sort_pos": 3},{"name": "Server", "icon": "dns", "sort_pos": 4}

[admin]
role = admin
password = 
confirm_password = 

[Home Assistant]
prefix = https://
url = hassio.owlnest.de
icon = static/images/apps/home-assistant.png
sidebar_icon = static/images/apps/home-assistant.png
description = Open source home automation that puts local control and privacy first
open_in = this_tab
tags = Home

[Mailcow]
prefix = https://
url = mail.owlnest.de
icon = static/images/apps/mailcow.png
sidebar_icon = static/images/apps/mailcow.png
description = mailcow is a mail server suite based on Dovecot, Postfix and other open source software, that provides a modern web UI for user/server administration.
open_in = this_tab
tags = Home

[ip-fetch]
platform = curl
resource = https://api.myip.com
value_template = <div class="row center-align"><div class="col s12"><h5><i class="material-icons-outlined" style="position:relative; top: 4px;">dns</i> My IP Address</h5><span class="theme-primary-text">{{value.ip}}</span></div></div>
response_type = json

[MyIp.com]
type = custom
data_sources = ip-fetch
tags = Home

[weather-fetch]
platform = weather
woeid = 680564

[Weather]
type = custom
data_sources = weather-fetch
tags = Home

[Nextcloud]
prefix = https://
url = cloud.owlnest.de
icon = static/images/apps/nextcloud.png
sidebar_icon = static/images/apps/nextcloud.png
description = A safe home for all your data – community-driven, free & open source
open_in = iframe
tags = Work

[BookStack]
prefix = https://
url = wiki.owlnest.de
icon = static/images/apps/bookstack.png
sidebar_icon = static/images/apps/bookstack.png
description = BookStack is a simple, self-hosted, easy-to-use platform for organising and storing information
open_in = this_tab
tags = Work

[VS Code]
prefix = https://
url = your-website.com
icon = static/images/apps/vscode.png
sidebar_icon = static/images/apps/vscode.png
description = A code editor redefined and optimized for building and debugging modern web and cloud applications
open_in = this_tab
tags = Work

[Calibre-Web]
prefix = https://
url = read.owlnest.de
icon = static/images/apps/calibre-web.png
sidebar_icon = static/images/apps/calibre-web.png
description = Calibre-Web is a web app providing a clean interface for browsing, reading and downloading eBooks using an existing Calibre database.
open_in = this_tab
tags = Work

[Piwigo]
prefix = https://
url = your-website.com
icon = static/images/apps/piwigo.png
description = Manage your photos with Piwigo, a full featured open source photo gallery application for the web.
open_in = this_tab
tags = Work

[PrivateBin]
prefix = https://
url = paste.owlnest.de
icon = static/images/apps/privatebin.png
sidebar_icon = static/images/apps/privatebin.png
description = PrivateBin is a minimalist, open source online pastebin where the server has zero knowledge of pasted data.
open_in = this_tab
tags = Work

[Search Engines]
type = collection
icon = collections_bookmark
urls = {"url": "https://google.com", "icon": "https://www.google.com/favicon.ico", "name": "Google", "open_in": "new_tab"},{"url": "https://duckduckgo.com", "icon": "https://duckduckgo.com/favicon.ico", "name": "DuckDuckGo", "open_in": "this_tab"},{"url": "https://qwant.com", "icon": "https://qwant.com/favicon.ico", "name": "Qwant", "open_in": "this_tab"},{"url": "https://startpage.com", "icon": "https://www.startpage.com/sp/cdn/favicons/favicon--default.ico", "name": "Startpage", "open_in": "this_tab"},{"url": "https://searx.xyz", "icon": "https://searx.xyz/favicon.ico", "name": "SearchX", "open_in": "this_tab"}
tags = Work

[Jellyfin]
prefix = https://
url = media.owlnest.de
icon = static/images/apps/jellyfin.png
sidebar_icon = static/images/apps/jellyfin.png
description = The Free Software Media System
open_in = this_tab
tags = Media

[Jackett]
prefix = https://
url = your-website.com
icon = static/images/apps/jackett.png
sidebar_icon = static/images/apps/jackett.png
description = API Support for your favorite torrent trackers
open_in = this_tab
tags = Media

[Radarr]
prefix = https://
url = radarr.owlnest.de
icon = static/images/apps/radarr.png
sidebar_icon = static/images/apps/radarr.png
description = A fork of Sonarr to work with movies à la Couchpotato
open_in = this_tab
tags = Media

[Sonarr]
prefix = https://
url = sonarr.owlnest.de
icon = static/images/apps/sonarr.png
sidebar_icon = static/images/apps/sonarr.png
description = Smart PVR for newsgroup and bittorrent users
open_in = this_tab
tags = Media

[Gitea]
prefix = https://
url = git.owlnest.de
icon = static/images/apps/gitea.png
sidebar_icon = static/images/apps/gitea.png
description = A painless self-hosted Git service
open_in = this_tab
tags = Server

[Syncthing]
prefix = https://
url = sync.owlnest.de
icon = static/images/apps/Syncthing.png
sidebar_icon = static/images/apps/Syncthing.png
description = Open Source Continuous File Synchronization
open_in = this_tab
tags = Server

[Proxmox]
prefix = https://
url = pve.owlnest.de
icon = static/images/apps/proxmox.png
sidebar_icon = static/images/apps/proxmox.png
description = Proxmox VE is a complete open-source platform for enterprise virtualization.
open_in = this_tab
tags = Server

[Nginx Proxy Manager]
prefix = https://
url = proxy.owlnest.de
icon = static/images/apps/nginxproxymanager.png
sidebar_icon = static/images/apps/nginxproxymanager.png
description = Docker container for managing Nginx proxy hosts with a simple, powerful interface
open_in = this_tab
tags = Server

[Portainer]
prefix = https://
url = portainer.owlnest.de
icon = static/images/apps/portainer.png
sidebar_icon = static/images/apps/portainer.png
description = Making Docker management easy
open_in = this_tab
tags = Server

[Pi-hole]
prefix = https://
url = pihole.owlnest.de
icon = static/images/apps/pihole.png
sidebar_icon = static/images/apps/pihole.png
description = A black hole for Internet advertisements
open_in = this_tab
tags = Server

[Grafana]
prefix = https://
url = monitor.owlnest.de
icon = static/images/apps/grafana.png
sidebar_icon = static/images/apps/grafana.png
description = The open observability platform
open_in = this_tab
tags = Server

[Jenkins]
prefix = https://
url = jenkins.owlnest.de
icon = static/images/apps/jenkins.png
sidebar_icon = static/images/apps/jenkins.png
description = Jenkins is an open source automation server which enables developers around the world to reliably build, test, and deploy their software.
open_in = this_tab
tags = Server

[Unifi]
prefix = https://
url = your-website.com
icon = https://www.colligso.com/images/integrations/unifi.png
sidebar_icon = https://www.colligso.com/images/integrations/unifi.png
description = The UniFi® Controller is a wireless network management software solution from Ubiquiti Networks™. It allows you to manage multiple wireless networks using a web browser.
open_in = this_tab
tags = Server

[Minecraft]
prefix = https://
url = mc1.lazylab.com
icon = https://www.baycroftschool.com/wp-content/uploads/2021/02/minecraft.png
sidebar_icon = https://www.baycroftschool.com/wp-content/uploads/2021/02/minecraft.png
description = Example description
open_in = iframe
tags = Server
```
