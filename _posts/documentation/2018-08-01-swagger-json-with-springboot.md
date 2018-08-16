---
layout: post
comments: true
title:  "swagger.json with Spring Boot"
date: 2018-08-01 17:45:09 -0700
categories: documentation
tags: 
- Swagger
- Spring Boot
description: swagger.json with Spring Boot
published: true
---

[Swagger](https://swagger.io/) 
> Swagger is the world’s largest framework of API developer tools for the OpenAPI Specification (OAS), enabling development across the entire API lifecycle, from design and documentation, to test and deployment.

[SpringFox](http://springfox.github.io/springfox/) 
> is a Swagger integration for the Spring Framework. It can automatically inspect your classes, detect Controllers, their methods, model classes they use and URLs to which they are mapped. Without any handwritten documentation, it can generate a lot of information about your API just by inspecting classes in your application.

This post is based allow on blog post from [Documenting Spring Boot REST API with Swagger and SpringFox](Documenting Spring Boot REST API with Swagger and SpringFox) from Vojtech Ruzicka. 

But I just want to get the swagger.json file, and won't setup or use SwaggerUI.

All you need to do is include the dependency
```xml
<dependency>
  <groupId>io.springfox</groupId>
  <artifactId>springfox-swagger2</artifactId>
  <version>2.9.2</version>
</dependency>
```

and include `@EnableSwagger2` annotation in your Spring project, for example add a config class 

```java
@Configuration
@EnableSwagger2
public class SpringFoxConfig {
  @Bean
  public Docket apiDocket() {
    return new Docket(DocumentationType.SWAGGER_2)
        .select()
        .apis(RequestHandlerSelectors.any())
        .paths(PathSelectors.any())
        .build();
  }
}
```

Then build and deploy your project you can view the swagger.json at `http://localhost:8080/v2/api-docs`

Vojtech Ruzicka goes into allot more detail on how to add SwaggerUI as well as customising your docs. 

The source code related to this blog post is available on [GitHub](https://github.com/melissapalmer/spring-boot-swagger-json).

References
====
- [https://www.vojtechruzicka.com/documenting-spring-boot-rest-api-swagger-springfox/](https://www.vojtechruzicka.com/documenting-spring-boot-rest-api-swagger-springfox/)