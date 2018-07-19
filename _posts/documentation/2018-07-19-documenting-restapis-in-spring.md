---
layout: post
title:  "Documentiong Rest APIs in Spring"
date: 2018-07-19 17:45:09 -0700
categories: documentation
tags: 
- Spring REST Docs
- SwaggerUI 
description: Documentiong Rest APIs in Spring.
---

Inspired by [Keycloak's Admin REST API](https://www.keycloak.org/docs-api/4.1/rest-api/index.html) I wanted to create documentation for my REST API that included: nice formatting of the models with as little extra effort above the coding as possible. As well as example request/respones and a Swagger like playground area. 

[Swagger](https://swagger.io/) although widely used and well known, requires you to add many annotations to your code base. Personally I found that all the Swagger annotations polluted the code which made it very difficult to read and maintain. For example take a look at this snippet of code below: 

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
```


**I set out to look for something that did not add any extra burden on the code.**

Looking around within the Java/Spring world there seem to been two options SwaggerUI and/or Spring REST Docs.

My project to experiment with is at: https://github.com/melissapalmer/springboot-restdoc-swagger

The Spring Boot RestDocs page is here: [https://melissapalmer.github.io/springboot-restdoc-swagger/]

References
====
- [https://www.keycloak.org/docs-api/4.1/rest-api/index.html](https://www.keycloak.org/docs-api/4.1/rest-api/index.html)
