---
layout: post
title: "Docker for Unix on Windows with Vagrant"
date: 2018-08-02 17:45:09 -0700
categories: docker
tags: 
- Vagrant
- Docker
description: Docker for Unix on Windows with Vagrant
published: true
---

Although there is [Docker for Windows](https://www.docker.com/docker-windows) now allowing windows users to user Docker. Docker for Windows can only be used for Docker images based on Windows OS... if you wanting to run Docker containers where the images are based on Linux OS [Docker for Windows](https://www.docker.com/docker-windows) won't work. 

To do this on Windows you'll need to have a Linux VM, onto which you can install [Docker for Ubuntu](https://www.docker.com/docker-ubuntu). 

**You'll need 3 things installed: **
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)	a program that allows creation of VMs (alternatives are VMware, Oracle VM)
- [Vagrant](https://www.vagrantup.com/downloads.html)		VMs initially have nothing installed on them. You need to install and configure them as if they were new computer. This is known as provisioning. Vagrant combines the powers of a provisioner and VirtualBox to configure VMs for you. (alternatives for provisioning are Ansible, Chef, Puppet)
- [PuTTY and PuTTYGen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)	ssh client (to login to Linux VM and generator of security keys)
All of these install like any other windows program.

**Set up your project with the following 3 files in the root directory: **
- Vagrantfile
- DockerHostVagrantfile
- Dockerfile

The Vagrantfile is the starting point. It's usually used to create VMs in VirtualBox, VMWare etc. buy using a different provider. You can now use the docker provider, Vagrant will spin up Docker containers rather than VMs for this provider. An example Vagrantfile looks as follows: 

```
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'
 
Vagrant.configure("2") do |config|

	config.vm.define "docker-4-unix-on-windows" do |m|	 
		m.vm.provision :shell, inline: "sudo apt-get update"

		m.vm.provider "docker" do |d|
			d.name = 'docker-4-unix-on-windows'
			
			d.force_host_vm = true
			d.has_ssh = true

			#this will look for a Dockerfile in the same directory as the Vagrantfile. 
			#When vagrant up --provider=docker is run, Vagrant automatically builds that Dockerfile and starts a container based on that Dockerfile
			
			d.build_dir = "." 
			d.cmd = ["ping", "-c 5151", "127.0.0.1"]
			d.remains_running = true
			
			d.vagrant_machine = "dockerhostvm"
			d.vagrant_vagrantfile = "./DockerHostVagrantfile"
		end
	end
end
```

Within the DockerHostVagrantfile you can specify the settings for you Host VM like any other Vagrantfile, as follows: 

```
Vagrant.configure("2") do |config|
  
  config.vm.provision "docker"

  # https://www.vagrantup.com/intro/getting-started/providers.html 
  # The following line terminates all ssh connections. Therefore
  # Vagrant will be forced to reconnect.
  # That's a workaround to have the docker command in the PATH
  config.vm.provision "shell", inline:
    "ps aux | grep 'sshd:' | awk '{print $2}' | xargs kill"

  config.vm.define "dockerhostvm"
  config.vm.box = "ubuntu/trusty64"
  config.vm.network "forwarded_port",
    guest: 8080, host: 8080
 
  config.vm.provider :virtualbox do |vb|
      vb.name = "dockerhostvm"
      
      #vb.customize ["modifyvm", :id, "--memory", "2048"]
      #vb.customize ["modifyvm", :id, "--cpus", "2"]
  end
end
```

And the Dockerfile is standard e.g.: 

```
FROM ubuntu:14.04
 
RUN mkdir /u01 && \
	chmod a+xr /u01
COPY /files/readme.txt /u01/
```

And the command to get everything running is then 
`vagrant up`
this will start provisioning, based on the Vagrantfile. Vagrant will realize that we asking for a Docker provider and that we on Windows which does not support Docker. Therefore, a Docker enabled host VM is required. Vagrant will then use the DockerHostVagrantfile because of the line `d.vagrant_vagrantfile = "./DockerHostVagrantfile"` to spin up a new VM in Virtualbox (as this file used `config.vm.provider :virtualbox`) and proceed to install Docker onto it. Vagrant will then build and run the Docker image as a container from the Dockerfile in same location. 

The line `d.cmd = ["ping", "-c 5151", "127.0.0.1"]` in the Vagrantfile tells the Docker container what to execute as soon as it is running. This is the same as the CMD within a normal Dockerfile. You can see the output from the container on the Windows host using the command `vagrant docker-logs`, or using Putty to SSH to onto the VM and running standard docker commands such as `docker logs <container-id>`.

To get the detail on VM to SSH to you can run `vagrant ssh-config` which will give you info. such as:

```
C:\work\projects\Learning\docker>vagrant ssh-config
Host docker-4-unix-on-windows
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile C:/Users/melissa.palmer/.vagrant.d/insecure_private_key
  IdentitiesOnly yes
  LogLevel FATAL
C:\work\projects\Learning\docker>
```

Here you can see the IP to use in Putty is 127.0.0.1 and Port 2222

You now have Docker running on a Linux Machine... on Windows.

**Vagrant Commands**
vagrant up
vagrant docker-logs
vagrant global-status

>Note that all vagrant commands by default act on the Docker container listed here as default. Vagrant destroy, halt, up etc. all act on that container and not on the dockerhostvm  Virtual Box VM. If you want vagrant to act on that machine, the commands need to make use of the id of the VM for example vagrant halt <machine id>.

**Docker Commands**
docker ps -a 
docker inspect -f {{ .NetworkSettings.IPAddress }} <container id>

Code for this example is on [GIT](https://www.sitepoint.com/getting-started-vagrant-windows/)

References
===

- [https://www.sitepoint.com/getting-started-vagrant-windows/](https://www.sitepoint.com/getting-started-vagrant-windows/)
- [https://technology.amis.nl/2015/08/22/first-steps-with-provisioning-of-docker-containers-using-vagrant-as-provider/](https://technology.amis.nl/2015/08/22/first-steps-with-provisioning-of-docker-containers-using-vagrant-as-provider/)
- [https://www.sitepoint.com/re-introducing-vagrant-right-way-start-php/](https://www.sitepoint.com/re-introducing-vagrant-right-way-start-php/)
- [https://www.vagrantup.com/docs/docker/basics.html](https://www.vagrantup.com/docs/docker/basics.html)
