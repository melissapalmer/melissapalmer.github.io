---
layout: post
title:  "Documenting Rest APIs"
date: 2018-07-19 17:45:09 -0700
categories: documentation
tags: 
- Spring REST Docs
- Swagger
description: Documenting Rest APIs.
---

Why is documentation important?
> APIs are only as good as their documentation. A great API can be rendered useless if people don’t know how to use it, which is why documentation can be crucial for success in the API economy. 

However, creating documentation isn't something that most developers enjoy doing of have the luxury of time to do. Creating and Maintaining documentation takes time and effort. As such I set out to find the best way of Automatize API Documentation to make this a more manageable process. There are a number of tools and frameworks which can help: each comes with its own set of pro's and cons. 

[Swagger](https://swagger.io/) is one of these. Although widely used and well known, like [Andy Wilkinson](https://spring.io/team/awilkinson) explains in his [presentation](https://2015.event.springone2gx.com/schedule/sessions/documenting_restful_apis.html) at the Washington DC, at SpringOne2GX conference or how [Carlos Barragan](https://blog.novatec-gmbh.de/the-problems-with-swagger/) details in this blog post it comes with its own pro’s and cons. Which include: 

* Annotation Hell

	Swagger requires you to add many annotations to your code base. These Swagger annotations pollute the code and make it very difficult to read and maintain. 

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

	![Swagger Annotations](/assets/images/documentation/SwaggerAnnotations.JPG)

* Structured in terms of URIs

	next reason description

[Spring REST Docs](https://spring.io/projects/spring-restdocs) 

Cons
* Having to put all descriptions for attributes into the Unit Tests

[Spring Auto REST Docs](https://dzone.com/articles/introducing-spring-auto-rest-docs)

Still missing the "play ground area SwaggerUI supplies"

References
====
- [https://www.keycloak.org/docs-api/4.1/rest-api/index.html](https://www.keycloak.org/docs-api/4.1/rest-api/index.html)
- [https://spring.io/projects/spring-restdocs](https://spring.io/projects/spring-restdocs) 
- [https://dzone.com/articles/introducing-spring-auto-rest-docs](https://dzone.com/articles/introducing-spring-auto-rest-docs)
- [http://www.baeldung.com/spring-rest-docs](http://www.baeldung.com/spring-rest-docs)
- [https://dzone.com/articles/best-practices-in-api-documentation?fromrel=true](https://dzone.com/articles/best-practices-in-api-documentation?fromrel=true)
- [https://blog.novatec-gmbh.de/the-problems-with-swagger/](https://blog.novatec-gmbh.de/the-problems-with-swagger/)
- [https://dzone.com/articles/swagger-great](https://dzone.com/articles/swagger-great)
- [https://springframework.guru/spring-boot-restful-api-documentation-with-swagger-2/](https://springframework.guru/spring-boot-restful-api-documentation-with-swagger-2/)
