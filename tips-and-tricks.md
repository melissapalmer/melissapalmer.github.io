---
layout: post
title:  "Ticks & Tricks"
---

# Overview

Various links to people's tips and tricks for tools. 



# Vagrant

`vagrant up --no-provision`

- [Vagrant tips and tricks](http://paweloczadly.github.io/devops/2014/10/22/vagrant-tips-and-tricks){:target="_blank"}
- Vagrant corrupted index file C:\Users\USERNAME\.vagrant.d/data/machine-index/index [https://stackoverflow.com/questions/24911021/vagrant-corrupted-index-file-c-users-username-vagrant-d-data-machine-index-ind](https://stackoverflow.com/questions/24911021/vagrant-corrupted-index-file-c-users-username-vagrant-d-data-machine-index-ind)

# Docker
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