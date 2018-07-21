---
layout: post
title:  "Documenting Rest APIs in Spring"
date: 2018-07-19 17:45:09 -0700
categories: documentation
tags: 
- Spring REST Docs
- SwaggerUI 
description: Documenting Rest APIs in Spring.
---

Why is documentation important?
> APIs are only as good as their documentation. A great API can be rendered useless if people don’t know how to use it, which is why documentation can be crucial for success in the API economy. 

However, creating documentation isn't something that most developers enjoy doing of have the luxury of time to do. Creating and Maintaining documentation takes time and effort. As such I set out to find the best way of Automatize API Documentation to make this a more manageable process. There are a number of tools and frameworks which can help: each comes with its own set of pro's and cons. 

[Swagger](https://swagger.io/) is one of these. Although widely used and well known, like [Andy Wilkinson](https://spring.io/team/awilkinson) explains in his [presentation](https://2015.event.springone2gx.com/schedule/sessions/documenting_restful_apis.html) at the Washington DC, at SpringOne2GX conference or how [Carlos Barragan](https://blog.novatec-gmbh.de/the-problems-with-swagger/) details in this blog post it comes with its own pro’s and cons. Which include: 

| Reason | Description |
| ------ | ----------- |
| "Annotation Hell"   | Swagger requires you to add many annotations to your code base. I found that all the Swagger annotations polluted the code which made the code very difficult to read and maintain. For example take a look at this snippet of code below: 
| Structured in terms of URIs   | next reason description |

My project to experiment with is at: https://github.com/melissapalmer/springboot-restdoc-swagger

The Spring Boot RestDocs page is here: [https://melissapalmer.github.io/springboot-restdoc-swagger/]

References
====
- [https://www.keycloak.org/docs-api/4.1/rest-api/index.html](https://www.keycloak.org/docs-api/4.1/rest-api/index.html)
- [Spring REST Docs ](https://spring.io/projects/spring-restdocs) 
- [Introduction to Spring Auto REST Docs](https://dzone.com/articles/introducing-spring-auto-rest-docs)
- [http://www.baeldung.com/spring-rest-docs](http://www.baeldung.com/spring-rest-docs)
- [https://dzone.com/articles/best-practices-in-api-documentation?fromrel=true]
- [https://blog.novatec-gmbh.de/the-problems-with-swagger/]
- https://dzone.com/articles/swagger-great 
