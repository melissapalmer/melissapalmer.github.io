---
layout: post
comments: true
title: "Newman with Docker"
date: 2018-08-31 17:45:09 -0700
categories: testing
tags: 
- Postman
- Docker
description: Newman with Docker
published: true
---

# Overview

[Postman](https://www.getpostman.com/) which is a popular API client for interacting with RESTFull APIs. Postman also includes the ability to create test suites and run these from its UI. 

![Postman UI](/assets/images/testing/Postman1.JPG)

You  can export these collections, to share with others and use elsewhere. 

Postman also has its own CLI ... [Newman](https://www.getpostman.com/docs/v6/postman/collection_runs/command_line_integration_with_newman) that allows for running of a Postman collection (test suites) from the command line. 

`newman run HelloWorld.postman_collection.json`

The thing is that to run this Newman command you also need Node.js installed along with the Newman npm package.  Newman has its own Docker image more details on that at: [Newman with Docker](https://www.getpostman.com/docs/v6/postman/collection_runs/newman_with_docker). To use image, with Docker, you can run a command like:

`docker run -v "$(pwd)":/tmp -t postman/newman_ubuntu1404 run /tmp/HelloWorld.postman_collection.json` 

# To test this out

I am going to: 

- Create a simple webapp, that'll allow us to have a URL we can hit and "test".
  - Use Docker to create an image, with Apache HTTP and a single webpage, saying Hello World!. 
- Run this webapp using Docker 
- Create a Postman test script to test this
- Run that script via Newman Docker image 

# Create a `Dockerfile`

Simply:

```dockerfile
FROM httpd:alpine
COPY ./public-html/ /usr/local/apache2/htdocs/
```

- Start from Apache HTTP image
- Copy contents of folder public-html to /apache2/htdocs/

Under folder public-html I have one file index.html, with: 

```html
Hello World!
```

# Build and Run this Docker image

```
docker build -t my-webapp .
docker run -dit --name helloworld -p 80:80 my-webapp
```

- The above creates an image named my-webapp
- Then uses this image, to start a container named helloworld

You should be able to run `curl http://localhost:80` and you will see Hello World! You could also run your Postman tests against this url using the Postman GUI. 

# Test webapp with Newman Docker

`docker run -v "$(pwd)":/tmp -t postman/newman_ubuntu1404 run /tmp/HelloWorld.postman_collection.json`

The above command is doing the following: 

-  Starting a Docker container from image postman/newman_ubuntu1404
- Maps the current directory to a folder called /tmp on the Docker image
- Which has the Newman CLI as it's entry point, so you can use all Newman command line parameters. These can be found at [Command line integration with Newman](https://www.getpostman.com/docs/v6/postman/collection_runs/command_line_integration_with_newman)
- above `run /tmp/HelloWorld.postman_collection.json` is passing a command to Newman to run the tests in collection /tmp/HelloWorld.postman_collection.json

**This will FAIL** ....: if you remember in our, Postman script  we used the hostname of localhost (ie: http://localhost:80/).

# Docker Network

We now have 2 Docker container running: my-running-webapp and postman/newman_ubuntu1404. The postman/newman_ubuntu1404 Docker container will attempt to run our collection HelloWorld.postman_collection.json and go to URL http://localhost:80/ which will fail with `connect ECONNREFUSED 127.0.0.1:80` 

This is because its trying to go to localhost on the container that's running, which has not webapp running on it. 

We want our containers to be able to communicate with each other. Remember each time a container starts up ... it'll get a new IP.  So using and IP instead of localhost will not help. 

Using a Docker network we can use container names as the host address. 

```
docker network create hello_world_network
```

Then include --network=hello_world_network when starting up our two containers. For example:

```
docker run --network=hello_world_network -dit --name helloworld -p 80:80 my-webapp
```

and

```
docker run --network=hello_world_network -v "$(pwd)":/tmp -t postman/newman_ubuntu1404 run /tmp/HelloWorld.postman_collection.json
```

THEN from the postman/newman_ubuntu1404 container we can  http://helloworld:80/ in Postman scripts. 

You should see the results of the test similar to below:

![Newman Results](/assets/images/testing/Postman2.JPG)

The code for this can be found on [GIT](https://github.com/melissapalmer/newman-with-docker)

# References

- [http://blog.getpostman.com/2015/07/30/official-docker-image-for-newman/](http://blog.getpostman.com/2015/07/30/official-docker-image-for-newman/)
- [https://github.com/postmanlabs/newman/tree/develop/docker](https://github.com/postmanlabs/newman/tree/develop/docker)
- [https://www.getpostman.com/docs/v6/postman/collection_runs/newman_with_docker](https://www.getpostman.com/docs/v6/postman/collection_runs/newman_with_docker)
- [https://blog.linuxserver.io/2017/10/17/using-docker-networks-for-better-inter-container-communication/](https://blog.linuxserver.io/2017/10/17/using-docker-networks-for-better-inter-container-communication/)

# Other related links
- [https://dzone.com/articles/how-to-get-command-line-integration-with-newman-in?fromrel=true](https://dzone.com/articles/how-to-get-command-line-integration-with-newman-in?fromrel=true)
- [https://dzone.com/articles/testing-apis-using-postman](https://dzone.com/articles/testing-apis-using-postman)
