---
layout: post
comments: true
title: "Docker Ticks and Tricks"
categories: tips and tricks
tags: 
- Docker
description: Docker
published: true
---

**SSH to docker container**     `docker exec -i -t nameofcontainer_or_containerid sh`

**Copying files from Docker container to host** 

`docker cp <containerId>:/file/path/within/container /host/path/target`

For example: 
`sudo docker cp nameofcontainer_or_containerid:/app.jar .`

**Get IP of docker container**
`docker inspect -f {% raw  %}'{{ range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}'{% raw  %} name_of_container`

# References

**Docker References**
- [Useful Docker Commands](https://blog.csainty.com/2016/07/useful-docker-commands.html)
- [Copying files from Docker container to host](https://stackoverflow.com/questions/22049212/copying-files-from-docker-container-to-host)