---
layout: post
comments: true
title: "Deploying a Spring Boot app to K8s"
date: 2018-12-16 08:45:09 -0700
categories: devops
tags: 
- Docker
- Kubernetes
- Helm
description: Deploying a Spring Boot app to K8s
published: true
---

# Deploying a Spring Boot app to K8s

In this post I create a greeting RESTFul API with Spring, that queries a DB for its hello strings. Use Docker to create an image and run it in a container. I'll cover how to use docker compose to run multiple containers for the application (our app, and postgres DB). Finally deploying the app to a K8s cluster using Helm package manager.

Thank you to **The Practical Developer** as is post at: https://thepracticaldeveloper.com/2017/12/11/dockerize-spring-boot/ allot of this is based off of what he taught is there. 

**I go through the following core technology stack, during this post:**

- [Spring](https://spring.io/) is an application framework and inversion of control container for the Java platform.
- [Docker](https://www.docker.com/)  is the container technology that allows you to containerise your applications.
- [Docker Compose](https://docs.docker.com/compose/) is a tool for defining and running multi-container applications. 
- [Kubernetes](https://kubernetes.io/) (commonly known as K8s) is an open-source container-orchestration system for automating deployment, scaling and management of containerised applications.
- [Helm](https://docs.helm.sh/) is a package manager for K8s, it simplifies the installation of an application and its dependencies into a K8s cluster.

# 01-Create a Spring Boot Rest API

For this step, I have assumed prior knowledge of Spring Boot, Maven and creating Restful APIs. If not checkout the following Spring.io guides: [Building a RESTful Web Service](https://spring.io/guides/gs/rest-service/), [Building REST services with Spring](https://spring.io/guides/tutorials/bookmarks/), [Accessing JPA Data with REST](https://spring.io/guides/gs/accessing-data-rest/)

You can also grab the code in my repo, on the 01-springboot-app branch in [GitHub](https://github.com/melissapalmer/basic-java-app-2-helm/tree/01-springboot-app)

**This application includes:** 

- An endpoint `/hello` that'll respond with various greetings text sourced from a DB. 
- Spring Actuator: which exposes health check endpoints, that will be used later on

**Few things to notice**

- application.yml includes the setting `spring.datasource.platform=h2` Spring in turn knows to use the  `data-h2.sql` file to initialise the h2 DB on start up. 
- I have done this so that later, we can initialise the 'real' DB in other ways. It will help see how a 'real' DB is used vs. the H2 in memory DB.

**Compiling and Running the app** 

- Build Using `./mvnw clean package`
- Run Using `java -jar target/docker-2-helm.jar` or `./mvnw spring-boot:run`

Go to http://localhost:8080/hello to see a hello message. 
Also test out the Spring Actuator endpoints: http://localhost:8080/actuator and http://localhost:8080/actuator/health

**So far we have done nothing with Docker**, however it is important to understand, that we've: 

- only built and run the application locally: **using a pre-installed version** of Java &/or Maven. 
- If we were to hand this over to the an OP's team at this point: 
  - we'd need to specify to them what version of Java is needed

# **02-Containerise It** 

This step takes the Spring Boot application and creates a Docker image. You'll need to ensure you have Docker installed.

## Prerequisites

- [Docker](https://docs.docker.com/v17.09/engine/installation/linux/docker-ce/ubuntu/)		Install Instructions: `sudo apt install docker`

Create a Dockerfile, as below, in your project. This is used to define an image, build it and run it using Docker commands.

```dockerfile
# Step : Build image
FROM maven:3.5.3-jdk-8-alpine as BUILD
WORKDIR /build
COPY pom.xml .
# get all the downloads out of the way
# mvn <maven-plugin-name>:help caches maven specific dependencies to image
# mvn dependency:go-offline caches build depencencies to image
RUN mvn clean
RUN mvn compiler:help jar:help resources:help surefire:help clean:help install:help deploy:help site:help dependency:help javadoc:help spring-boot:help
RUN mvn dependency:go-offline
COPY src/ /build/src/
RUN mvn package

# Step : Package image
FROM openjdk:8-jre-alpine as APP
EXPOSE 8080
COPY --from=BUILD /build/target/docker-2-helm.jar app.jar
#To reduce Tomcat startup time we added a system property pointing to "/dev/urandom" as a source of entropy.
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```
