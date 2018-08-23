---
layout: post
comments: true
title: "Newman with Docker on Windows with Vagrant"
date: 2018-08-05 17:45:09 -0700
categories: devops
tags: 
- Postman
- Docker
- Vagrant
description: Newman with Docker on Windows with Vagrant
published: false
---

[Postman](https://www.getpostman.com/) which is a popular API client for interacting with RESTFull APIs. Postman also includes the ability to create test suites and run these from its UI. Postman also has its own CLI ... [Newman](https://www.getpostman.com/docs/v6/postman/collection_runs/command_line_integration_with_newman) that allows for running of a Postman collection (test suites) from the command line. 

There are allot of tutorials out there explain how to use Newman & Jenkins. The thing is that all of these examples require you to have a Jenkins master/slave that has Node.js installed along with the Newman npm package. 

# Newman has its own Docker Image
more details on that at: [Newman with Docker](https://www.getpostman.com/docs/v6/postman/collection_runs/newman_with_docker). Using this image its as simple as running the a command like:

`docker run -t postman/newman_ubuntu1404 GOOGLE.postman_collection.json` 

to run your Postman Test Scripts with Newman and the output displayed in the console. The entry point to the Docker image is Newman. So you can use all Newman command line parameters. These can be found at [Command line integration with Newman](https://www.getpostman.com/docs/v6/postman/collection_runs/command_line_integration_with_newman)

Running the above command will output to the console (ie: docker logs) ... which is the same output you would see if running Newman on its own: 

```
newman

GOOGLE

→ ViewHomePage
  GET https://www.google.com [200 OK, 12.55KB, 854ms]
  ✓  Status code is 200
  ✓  Response time is less than 5000ms

┌─────────────────────────┬──────────┬──────────┐
│                         │ executed │   failed │
├─────────────────────────┼──────────┼──────────┤
│              iterations │        1 │        0 │
├─────────────────────────┼──────────┼──────────┤
│                requests │        1 │        0 │
├─────────────────────────┼──────────┼──────────┤
│            test-scripts │        1 │        0 │
├─────────────────────────┼──────────┼──────────┤
│      prerequest-scripts │        0 │        0 │
├─────────────────────────┼──────────┼──────────┤
│              assertions │        2 │        0 │
├─────────────────────────┴──────────┴──────────┤
│ total run duration: 1344ms                    │
├───────────────────────────────────────────────┤
│ total data received: 11.85KB (approx)         │
├───────────────────────────────────────────────┤
│ average response time: 854ms                  │
└───────────────────────────────────────────────┘
```

# I wanted to see if we can take advantage of Newman's Docker Image 
to do the same thing as all these tutorials BUT avoid having to install Node.js or Newman on a Jenkins master/slave. To get started, I'll just use the Newman Docker Image without Jenkins. 

I work on a Windows PC... so here I explain how to use Windows 10, Vagrant to run a Postman collection with Newman in a Docker container. The Vagrant steps are not needed, but help me to spin up a Linux VM with Docker installed, that I can work against to run Newman Docker Image. 

I'm assuming that those reading this are familiar with [Docker](https://www.docker.com/) and [Vagrant](https://www.vagrantup.com/), as I use these two software applications in this post. If not there are some great getting started articles at: 
- [https://www.docker.com/what-docker](https://www.docker.com/what-docker)
- [https://docs.docker.com/get-started/](https://docs.docker.com/get-started/)
- [https://www.vagrantup.com/intro/index.html](https://www.vagrantup.com/intro/index.html) 
- and other posts on my own site at: [https://melissapalmer.github.io/devops.html](https://melissapalmer.github.io/devops.html)

# On Windows
You can use Vagrant to spin up a VM with Docker installed, and execute the docker run commands against it. With Vagrant when you specify the provision'er as docker and vagrant_vagrantfile, Vagrant ensures there is a Host VM with Docker installed. In the post at [Docker for Unix on Windows with Vagrant](https://melissapalmer.github.io/devops/2018/08/03/docker-4-unix-on-windows.html), I explain a basic example of how to run Vagrant, VirtualBox, Docker on Windows.

# The Vagrantfile will look as below:

```
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'
 
Vagrant.configure("2") do |config|
 
  config.vm.define "docker-4-newman" do |m|
		
    m.vm.provider "docker" do |d|	
      d.name = 'docker-4-newman'
			
      d.force_host_vm = true
      d.has_ssh = true			
		    
      d.image = "postman/newman_ubuntu1404"
      d.volumes = ["/home/vagrant/src:/vagrant/newman-with-docker-on-windows"]
      d.cmd = ["run", "/vagrant/newman-with-docker-on-windows/GOOGLE.postman_collection.json", "--environment=/vagrant/newman-with-docker-on-windows/GOOGLE_ENV.postman_environment.json", "--reporters", "cli,junit,html", "--reporter-junit-export", "/vagrant/newman-with-docker-on-windows/newman-report.xml", "--reporter-html-export", "/vagrant/newman-with-docker-on-windows/outputfile.html" ]
			
      d.vagrant_machine = "dockerhostvm"
      d.vagrant_vagrantfile = "DockerHostVagrantfile"
    end
  end
end
```

# and the DockerHostVagrantfile is as: 

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
	
  config.vm.synced_folder ".", "/home/vagrant/src"
  
  config.vm.provider :virtualbox do |vb|
      vb.name = "dockerhostvm"
  end
 
end
```

# Where: 

- `DockerHostVagrantfile` contains information about the VM which will be your Host VM that has Docker installed on it. 
- `Vagrantfile` is the setup for the Docker commands you want to run

- `d.image` is the Docker image that you want to use
- `d.cmd` is the command to run against the Docker image. In this case we use the commands for Newman (the Newman docs are at [https://github.com/postmanlabs/newman](https://github.com/postmanlabs/newman))
- `d.volumes` to map the sync drives from your VM Docker Host (which was created by DockerHostVagrantfile specified by `d.vagrant_vagrantfile`)

- *NOTE:* be careful with the Vagrant's 'synced_folder' and Docker volumes. 
	-- Firstly in the DockerHostVagrantfile with line `config.vm.synced_folder ".", "/home/vagrant/src"` you are sync'ing you files from Windows PC to the Linux VM being span up that'll have Docker installed for you (i.e.: dockerhostvm) 
	-- Then in the Vagrantfile the `d.volumes` is to map folders from the Linux VM (dockerhostvm) to Docker Container, so that the container can see files on your Linux VM which have been synced with Windows PC. 
(I did have couple of issues working with these two. Have not finalised what is the 'correct way' and have an outstanding question at: [https://stackoverflow.com/questions/51696559/right-way-to-use-vagrant-vagrant-vagrantfile-synced-folder-to-docker-volumes](https://stackoverflow.com/questions/51696559/right-way-to-use-vagrant-vagrant-vagrantfile-synced-folder-to-docker-volumes))

## d.cmd Newman commands that we'll use include: 

- `--environment` allowing you to specify which Postman Environment file to use, when running the tests. 
- `--reporters` to select which output types you want after running the tests for example: cli, html, junit
-- allow with each of the matching report export parameters such as `--reporter-html-export` which is used to specify where the output of html report will be saved.
-- allow with each of the matching report export parameters such as `--reporter-junit-export` which is used to specify where the output of xml report will be saved.  

# Once all the above is setup 

... all that is left to do is run the command 

`vagrant up`

This will run your docker run command on VM host, command that gets run is: 

`docker run -v "$(pwd)":/tmp postman/newman_ubuntu1404 run /tmp/GOOGLE.postman_collection.json --environment=/tmp/GOOGLE_ENV.postman_environment.json --reporters cli,junit,html --reporter-junit-export /tmp/newman-report.xml --reporter-html-export /tmp/outputfile.html`

which will run the postman collection GOOGLE.postman_collection.json using environment variables in GOOGLE_ENV.postman_environment.json and output the results to the command line, as well as to an html file and xml file under /vagrant/newman-with-docker-on-windows/ folder. Which we will be able to see on our Windows machine as its been synced with Docker via volumns and Vagrant via synced folders. 

The code for this can be found on [GIT](https://github.com/melissapalmer/newman-with-docker-on-windows)

References
===

- [http://blog.getpostman.com/2015/07/30/official-docker-image-for-newman/](http://blog.getpostman.com/2015/07/30/official-docker-image-for-newman/)
- [https://github.com/postmanlabs/newman/tree/develop/docker](https://github.com/postmanlabs/newman/tree/develop/docker)
- [https://www.getpostman.com/docs/v6/postman/collection_runs/newman_with_docker](https://www.getpostman.com/docs/v6/postman/collection_runs/newman_with_docker)

- [https://dzone.com/articles/how-to-get-command-line-integration-with-newman-in?fromrel=true](https://dzone.com/articles/how-to-get-command-line-integration-with-newman-in?fromrel=true)
- [https://dzone.com/articles/testing-apis-using-postman](https://dzone.com/articles/testing-apis-using-postman)


http://blog.hde.co.jp/entry/2015/12/02/120006
We also configure the share-folder between the local machine and the Docker host machine over here.
