---
layout: post
comments: true
title: "Docker Ticks and Tricks"
date: 2019-03-04 17:45:09 -0700
categories: tipsandtricks
tags: 
- Docker
description: Docker Ticks and Tricks
published: true
---

**SSH to docker container**

`docker exec -i -t nameofcontainer_or_containerid sh`

**Copying files from Docker container to host**

`docker cp <containerId>:/file/path/within/container /host/path/target`
For example: 
`sudo docker cp nameofcontainer_or_containerid:/app.jar .`


# References

**Docker References**
- [Useful Docker Commands](https://blog.csainty.com/2016/07/useful-docker-commands.html)
- [Copying files from Docker container to host](https://stackoverflow.com/questions/22049212/copying-files-from-docker-container-to-host)