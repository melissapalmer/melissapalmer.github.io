---
layout: post
title:  "Docker on Virtualbox VM"
date: 2018-07-19 17:45:09 -0700
categories: docker
tags: 
- Docker
- Virtualbox
description: Docker on Virtualbox VM
published: false
---

Check that docker is installed and running
- sudo systemctl status docker

- docker ps
- docker run -d --name=api-umbrella -p 80:80 -p 443:443 -v "$(pwd)/config":/etc/api-umbrella nrel/api-umbrella

How to get a Docker container's IP address from the host?
- docker inspect <container id> | grep "IPAddress"
- docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name_or_id

To get all container names and their IP addresses in just one single command.
- docker inspect -f '{{.Name}} - {{.NetworkSettings.IPAddress }}' $(docker ps -aq)


References
====
- [https://stackoverflow.com/questions/17157721/how-to-get-a-docker-containers-ip-address-from-the-host?page=1&tab=votes#tab-top](https://stackoverflow.com/questions/17157721/how-to-get-a-docker-containers-ip-address-from-the-host?page=1&tab=votes#tab-top)
- [https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04)
- [https://stackoverflow.com/questions/51621813/ip-of-docker-container-virtualbox-windows](https://stackoverflow.com/questions/51621813/ip-of-docker-container-virtualbox-windows)

- [https://weblogs.asp.net/pglavich/getting-docker-running-on-my-windows-system](https://weblogs.asp.net/pglavich/getting-docker-running-on-my-windows-system)