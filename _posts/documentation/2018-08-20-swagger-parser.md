---
layout: post
title:  "Parsing swagger.json with Swagger Parser"
date: 2018-07-26 17:45:09 -0700
categories: documentation
tags: 
- Swagger Parser
- Swagger
description: Parsing swagger.json with Swagger Parser
---

I wanted to see if we can get all the Endpoints with HttpMethods for a Swagger Spec from the swagger.json.  
[Swagger Parser](https://github.com/swagger-api/swagger-parser) provides a way of reading a swagger.json file into Java POJOs.

You need to include the depenceny in your pom.xml as follows
```xml
    <dependency>
	    <groupId>io.swagger.parser.v3</groupId>
	    <artifactId>swagger-parser</artifactId>
	    <version>2.0.1</version>
	</dependency>
```

And then you can parse the file using (where petstore-swagger.json is in your class path): 
```java

	String swaggerAsString = ClasspathHelper.loadFileFromClasspath("petstore-swagger.json");
	Swagger swagger = new SwaggerParser().parse(swaggerAsString);
```

Code can be found on [GIT](https://github.com/melissapalmer/swagger-parser-java)

References
===

- [https://github.com/swagger-api/swagger-parser](https://github.com/swagger-api/swagger-parser)