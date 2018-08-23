---
layout: post
comments: true
title: "Vagrant docker-compose"
date: 2018-08-23 08:45:09 -0700
categories: devops
tags: 
- Vagrant
description: Vagrant docker-compose
published: true
---

# Overview

I primarily work on Windows and wanted to get Docker Compose running, without having to install Docker for Windows. **Vagrant to the rescue**... with docker its provider and vagrant-docker-compose plugin. 

In this post we will be using Vagrant to spin up a base VM, provision it with docker and docker-compose and then run all the Docker containers. 

**All you need to do is:**

- Install a vagrant plugin for `vagrant-docker-compose`
- Create a basic `Vagrantfile` with provisioner for `docker` and `docker-compose`
- Create a basic `docker-compose.yml` 
- Run `vagrant up` 

# Install vagrant-docker-compose plugin: 

```
vagrant plugin install vagrant-docker-compose
```

# Create Vagrantfile

```
Vagrant.configure("2") do |config|
  
  config.vm.box = "ubuntu/xenial64"
  
  #map ports from local PC, to VM ... which intern will be mapped to container ports in docker-compose.yml
  config.vm.network "forwarded_port", 
	guest: 8080, host: 8080
  config.vm.network "forwarded_port", 
	guest: 5433, host: 5433
	
  #allow access on windows to VM	
  config.vm.network "public_network", bridge: "Intel(R) Dual Band Wireless-AC 7265"
  
  # Mount this folder as RO in the guest, since it contains secure stuff
  config.vm.synced_folder ".", "/vagrant", :mount_options => ["ro"]

  #To install, rebuild and run docker-compose on vagrant up
  config.vm.provision :docker 
  config.vm.provision :docker_compose, yml: "/vagrant/docker-compose.yml", rebuild: true, run: "always"
end
```


# Create docker-compose.yml

```
version: '3.1'

volumes:
  init.sql: 
  restore-db.sh:    
  data:
  postgres_data:
    driver: local

services:
  db:
    image: postgres:9.6.9
    volumes:
    - postgres_data:/var/lib/postgresql/data #ensure you data is saved between container shutdown/startup
    - ./init.sql:/docker-entrypoint-initdb.d/init.sql #run SQL on postgres startup
    - ./restore-db.sh:/docker-entrypoint-initdb.d/restore-db.sh #run batch script on postgres startup (to import dump file into PG)
    - ./data:/data
    ports:
    - "5433:5432"
    environment:
    - POSTGRES_PASSWORD=example
    
  adminer:
    image: adminer
    restart: always
    ports:
    - 8080:8080
```

# Run 

```
vagrant up
```

# Test

By going to [http://localhost:8080/](http://localhost:8080/) you should see the Adminer login page. Login with postgres/example to see the tables of DB. 

![Adminer](/assets/images/devops/Adminer.JPG)

# What just happened? 

We have just used Vagrant 
- to spin up a base VM, 
- provision it with docker and docker-compose 
- and then run the docker-compose command against our docker-composed.yml 
- which has then intern spin number a 2 containers which we can interact with. 

# References

- [https://medium.com/@cnadeau_/vagrant-as-a-development-environment-e5a83010fb49](https://medium.com/@cnadeau_/vagrant-as-a-development-environment-e5a83010fb49)
- [https://github.com/leighmcculloch/vagrant-docker-compose](https://github.com/leighmcculloch/vagrant-docker-compose)
- [https://gist.github.com/mcharytoniuk/0388cbc5d2e0afacd1e4](https://gist.github.com/mcharytoniuk/0388cbc5d2e0afacd1e4)

