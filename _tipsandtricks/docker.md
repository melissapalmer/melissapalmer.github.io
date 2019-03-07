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

##### Table of Contents 
[SSH to docker container](#ssh_2_docker_container)  
[Copying files from Docker container to host](#copy_files_from_container)
[Get IP of docker container*](#get_id_of_container)  

<a name="ssh_2_docker_container"/>
## SSH to docker container
`docker exec -i -t nameofcontainer_or_containerid sh`

<a name="copy_files_from_container"/>
## Copying files from Docker container to host 

`docker cp <containerId>:/file/path/within/container /host/path/target`

For example: 
`sudo docker cp nameofcontainer_or_containerid:/app.jar .`

<a name="get_id_of_container"/>
## Get IP of docker container
{% raw  %}
`docker inspect -f '{{ range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' name_of_container`
{% endraw %}

<a name="references"/>
## References
- [Useful Docker Commands](https://blog.csainty.com/2016/07/useful-docker-commands.html)
- [Copying files from Docker container to host](https://stackoverflow.com/questions/22049212/copying-files-from-docker-container-to-host)