---
layout: post
comments: true
title:  "Parse swagger.json to Java"
date: 2018-08-02 17:45:09 -0700
categories: documentation
tags: 
- Swagger Parser
- Swagger
description: Parse swagger.json to Java
---

*I wanted to see if we can get all the Endpoints with HttpMethods for a Swagger Spec from the swagger.json.*  

[Swagger Parser](https://github.com/swagger-api/swagger-parser) provides a way of reading a swagger.json file into Java POJOs.

You need to include the depenceny in your pom.xml as follows
{% highlight xml %}
<dependency>
    <groupId>io.swagger.parser.v3</groupId>
    <artifactId>swagger-parser</artifactId>
    <version>2.0.1</version>
</dependency>
{% endhighlight %}

And then you can parse the file using: 
{% highlight java %}
String swaggerAsString = ClasspathHelper.loadFileFromClasspath("petstore-swagger.json"); //where petstore-swagger.json is in your class path
Swagger swagger = new SwaggerParser().parse(swaggerAsString);
{% endhighlight %}

To get all endpoints:
{% highlight java %}
Map<String, Path> paths = swagger.getPaths();
Set<String> endpoints = paths.keySet();
{% endhighlight %}

and the a available HttpMethods on these endpoints:
{% highlight java %}
Map<HttpMethod, Operation> operationMap = path.getOperationMap();
Set<HttpMethod> operationsKeySet = operationMap.keySet();
{% endhighlight %}

Code can be found on [GIT](https://github.com/melissapalmer/swagger-parser-java)

References
===

- [https://github.com/swagger-api/swagger-parser](https://github.com/swagger-api/swagger-parser)