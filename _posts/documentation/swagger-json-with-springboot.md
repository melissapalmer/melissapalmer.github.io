---
layout: post
title:  "swagger.json with Spring Boot"
date: 2018-08-01 17:45:09 -0700
categories: documentation
tags: 
- Swagger
- Spring Boot
description: swagger.json with Spring Boot
published: true
---

One of the most popular API documentation specifications is OpenApi, formerly known as Swagger. It allows you to describe your API's properties using either JSON or YAML metadata. 

It is a Swagger integration for Spring Framework. It can automatically inspect your classes, detect Controllers, their methods, model classes they use and URLs to which they are mapped. Without any handwritten documentation, it can generate a lot of information about your API just by inspecting classes in your application.

View the swagger.json at `http://localhost:8080/v2/api-docs`

The source code related to this blog post is available on [GitHub](https://github.com/melissapalmer/spring-boot-swagger-json).

References
====
- [https://www.vojtechruzicka.com/documenting-spring-boot-rest-api-swagger-springfox/](https://www.vojtechruzicka.com/documenting-spring-boot-rest-api-swagger-springfox/)
- [https://github.com/conorroche/swagger-doclet](https://github.com/conorroche/swagger-doclet)
