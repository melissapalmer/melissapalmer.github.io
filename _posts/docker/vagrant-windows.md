---
layout: post
title: "Docker for Unix on Windows with VirtualBox"
date: 2018-08-02 17:45:09 -0700
categories: docker
tags: 
- Vagrant
- Docker
description: Docker for Unix on Windows with VirtualBox
published: false
---

On Windows .. 
Use a VM to install Docker on and act as your Docker Host. 

**You need a tow things installed: **
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)	a program that allows creation of VMs (alternatives are VMware, Oracle VM)
- [Vagrant](https://www.vagrantup.com/downloads.html)		VMs initially have nothing installed on them. You need to install and configure them as if they were new computer. This is known as provisioning. Vagrant combines the powers of a provisioner and VirtualBox to configure VMs for you. (alternatives for provisioning are Ansible, Chef, Puppet)
- [PuTTY and PuTTYGen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)	ssh client (to login to Linux VM and generator of security keys)
All of these install like any other windows program.

**You need 3 files: **
- Vagrant
- DockerHostVagrantfile
- Dockerfile

**Vagrant Commands**
vagrant up
vagrant docker-logs
vagrant global-status

Note that all vagrant commands by default act on the Docker container – listed here as default. Vagrant destroy, halt, up etc. all act on that container and not on the dockerhostvm  Virtual Box VM. If you want vagrant to act on that machine, the commands need to make use of the id of the VM – for example vagrant halt <machine id>.

**Docker Commands**
docker ps -a 
docker inspect -f ‘{{ .NetworkSettings.IPAddress }}’ <container id>

References
===

- [https://www.sitepoint.com/getting-started-vagrant-windows/](https://www.sitepoint.com/getting-started-vagrant-windows/)
- [https://technology.amis.nl/2015/08/22/first-steps-with-provisioning-of-docker-containers-using-vagrant-as-provider/](https://technology.amis.nl/2015/08/22/first-steps-with-provisioning-of-docker-containers-using-vagrant-as-provider/)
- [https://www.sitepoint.com/re-introducing-vagrant-right-way-start-php/](https://www.sitepoint.com/re-introducing-vagrant-right-way-start-php/)


- [https://stackoverflow.com/questions/51621813/ip-of-docker-container-virtualbox-windows](https://stackoverflow.com/questions/51621813/ip-of-docker-container-virtualbox-windows)





- [https://www.sitepoint.com/getting-started-vagrant-windows/](https://www.sitepoint.com/getting-started-vagrant-windows/)
- [http://activelamp.com/blog/devops/docker-with-vagrant/](http://activelamp.com/blog/devops/docker-with-vagrant/)
- [https://technology.amis.nl/2015/08/22/first-steps-with-provisioning-of-docker-containers-using-vagrant-as-provider/](https://technology.amis.nl/2015/08/22/first-steps-with-provisioning-of-docker-containers-using-vagrant-as-provider/)
- [https://blog.zenika.com/2014/10/07/setting-up-a-development-environment-using-docker-and-vagrant/](https://blog.zenika.com/2014/10/07/setting-up-a-development-environment-using-docker-and-vagrant/)
- [https://blog.zencoffee.org/2016/08/ansible-vagrant-windows/](https://blog.zencoffee.org/2016/08/ansible-vagrant-windows/)
- [https://spring.io/guides/gs/spring-boot-docker/](https://spring.io/guides/gs/spring-boot-docker/)

- [http://fizzylogic.nl/2015/01/27/building-containerized-apps-with-vagrant/](http://fizzylogic.nl/2015/01/27/building-containerized-apps-with-vagrant/)
- [https://blog.jayway.com/2015/03/21/a-not-very-short-introduction-to-docker/](https://blog.jayway.com/2015/03/21/a-not-very-short-introduction-to-docker/)