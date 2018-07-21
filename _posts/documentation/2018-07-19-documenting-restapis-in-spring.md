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

> APIs are only as good as their documentation. A great API can be rendered useless if people don’t know how to use it, which is why documentation can be crucial for success in the API economy. 

However, creating documentation isn't something that most developers enjoy doing of have the luxury of time to include. Creating and Maintaining documentation takes effort and patience. 

As such I set out to find the best way of Automatize API Documentation to make this a more manageable process. There are a number of tools and frameworks which can help: each comes with its own set of pro's and cons. 

[Swagger](https://swagger.io/) is one of these. Although widely used and well known, like [Andy Wilkinson](https://spring.io/team/awilkinson) explains in his [presentation](https://2015.event.springone2gx.com/schedule/sessions/documenting_restful_apis.html) at the Washington DC, at SpringOne2GX conference or how [Carlos Barragan](https://blog.novatec-gmbh.de/the-problems-with-swagger/) details in this blog post it comes with its own pro’s and cons. Which include: 

| Option | Description |
| ------ | ----------- |
| "Annotation Hell"   | Swagger requires you to add many annotations to your code base. I found that all the Swagger annotations polluted the code which made the code very difficult to read and maintain. For example take a look at this snippet of code below: 

```java
@Produces( { MediaType.APPLICATION_JSON } )
@Path( "/{email}" )
@GET
@ApiOperation( 
    value = "Find person by e-mail", 
    notes = "Find person by e-mail", 
    response = Person.class 
)
@ApiResponses( {
    @ApiResponse( code = 404, message = "Person with such e-mail doesn't exists" )    
} )
public Person getPeople( 
        @ApiParam( value = "E-Mail address to lookup for", required = true ) 
        @PathParam( "email" ) final String email ) {
    // ...
}

@ApiModel( value = "Person", description = "Person resource representation" )
public class Person {
    @ApiModelProperty( value = "Person's first name", required = true ) 
    private String email;
    @ApiModelProperty( value = "Person's e-mail address", required = true ) 
    private String firstName;
    @ApiModelProperty( value = "Person's last name", required = true ) 
    private String lastName;
    // ...
}
``` |
| Another reason | next reason description |


Documenting Restful APIs
 

Inspired by [Keycloak's Admin REST API](https://www.keycloak.org/docs-api/4.1/rest-api/index.html) which includes object definitions for JSON body requests of the API. I wanted to create documentation for my REST API that included: nice formatting of the models with as little extra effort above the coding as possible. As well as example request/responses and a Swagger like playground area. 





Spring REST Docs, has a different strategy
> One major philosophy behind the project is the use of tests to produce the documentation. This ensures that the documentation always generated accurately matches the actual behavior of the API. 
> It combines hand-written documentation written with Asciidoctor and auto-generated snippets produced with Spring MVC Test. 
> This approach frees you from the limitations of the documentation produced by tools like Swagger.

Unfortunately there is still extra coding and steps that are needed to actually complete documenting the API. For example

![View Sublime Spelling Error](/assets/images/documentation/sublime-spelling-error.JPG)


**As such... I set out to look for something that did not add any extra burden on the code.**

[Spring REST Docs ](https://spring.io/projects/spring-restdocs) provides ease of combining hand-written documentation written and auto-generated snippets produced by unit tests. "It helps you to produce documentation that is accurate" with minimal extra effort. However there still requires you to code in the request/response JSON fields. As below: 


[Spring Auto REST Docs extension](https://scacap.github.io/spring-auto-restdocs/) automates the "recognition and documentation of request and response parameters." and picks up the JavaDoc rather than Swagger annotations to include in documentation. It's also able to automatically pick up the Java's Constraints from annotated fields such as @NotNull.


My project to experiment with is at: https://github.com/melissapalmer/springboot-restdoc-swagger

The Spring Boot RestDocs page is here: [https://melissapalmer.github.io/springboot-restdoc-swagger/]

References
====
- [https://www.keycloak.org/docs-api/4.1/rest-api/index.html](https://www.keycloak.org/docs-api/4.1/rest-api/index.html)
- [Spring REST Docs ](https://spring.io/projects/spring-restdocs) 
- [Introduction to Spring Auto REST Docs](https://dzone.com/articles/introducing-spring-auto-rest-docs)
- [http://www.baeldung.com/spring-rest-docs](http://www.baeldung.com/spring-rest-docs)
- [https://dzone.com/articles/best-practices-in-api-documentation?fromrel=true](https://dzone.com/articles/best-practices-in-api-documentation?fromrel=true)
- [https://blog.novatec-gmbh.de/the-problems-with-swagger/]
- [https://dzone.com/articles/swagger-great] 