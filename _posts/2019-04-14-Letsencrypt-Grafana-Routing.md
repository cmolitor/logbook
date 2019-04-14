---
layout:     post
title:      Using SSL for Grafana
author:     Christoph Molitor
tags:       Grafana SSL CheatSheet
subtitle:   Using Grafana on my own server via https
category:   report
---
<!-- Start Writing Below in Markdown -->

# Securing my Grafana dashboards with https using letsencrypt.org

## Initial setup 
Actually I followed the description of the following website (in german):
[Freifunk Winterberg](https://www.freifunk-winterberg.net/grafana-per-letsencrypt-mit-https-absichern/)

The procedure describes the setup of grafana using a SSL certificate provided by letsencrypt.org

## Renewal of certificate

Once in while (I don't remember the exact duration), you have to renew your certifcate by using the following command:

```
sudo certbot certonly
```

## Routing of incoming https traffic to grafana port (port 3000)

In order to redirect the https traffic to the grafana port (port 3000), you have to configure the routing with the followig command, which redirects traffic at port 443 (https) to port 3000 (grafana).
```
sudo iptables -t nat -A PREROUTING -p tcp --dport 443 -j REDIRECT --to-port 3000
```

If you like to check your routing table, sue the following command:
```
iptables -t nat -L --line-numbers
```

If you want to delete some set routing, e.g in line number 1, use the following command:
```
iptables -t nat -D PREROUTING 1
```