---
layout: post
comments: true
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

However, creating documentation isn't something that most developers enjoy doing of have the luxury of time to do. Creating and Maintaining documentation takes time and effort. 

As such I set out to find the best way of Automatize API Documentation to make this a more manageable process. There are a number of tools and frameworks which can help: each comes with its own set of pro's and cons. 

[Swagger](https://swagger.io/) is one of these. Although widely used and well known, like [Andy Wilkinson](https://spring.io/team/awilkinson) explains in his [presentation](https://www.youtube.com/watch?v=k5ncCJBarRI) at the Washington DC, at SpringOne2GX conference or how [Carlos Barragan](https://blog.novatec-gmbh.de/the-problems-with-swagger/) details in this blog post it comes with its own pro’s and cons. Which include: 

* __Annotation Hell__

	Swagger requires you to add many annotations to your code base. These Swagger annotations pollute the code and make it very difficult to read and maintain. You end up having more annotations than actual code... have a look below. 

{% highlight java %}
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
{% endhighlight %}

* __Swagger focuses heavily on the URIs.__

	Docs structured by URI. Force the consumers read through the URI and figure out you are talking about by themselves. 

[Spring REST Docs](https://spring.io/projects/spring-restdocs) 
> combines hand-written documentation written with Asciidoctor and auto-generated snippets produced with Spring MVC Test. This approach frees you from the limitations of the documentation produced by tools like Swagger. It helps you to produce documentation that is accurate, concise, and well-structured. This documentation then allows your users to get the information they need with a minimum of fuss.

it too also comes with its pro's and con's

* __Manual documentation of request and response JSON fields__
	
	You have to put all descriptions for attributes into the Unit Tests

{% highlight java %}
@Test
public void listPeople() throws Exception {
    createSamplePerson("George", "King");
    createSamplePerson("Mary", "Queen");

    this.document.snippets(
            responseFields(
                    fieldWithPath("[].id").description("The persons' ID"),
                    fieldWithPath("[].firstName").description("The persons' first name"),
                    fieldWithPath("[].lastName").description("The persons' last name")
            )
    );

    this.mockMvc.perform(
            get("/people").accept(MediaType.APPLICATION_JSON)
    ).andExpect(status().isOk());
}
{% endhighlight %}


This is not only cumbersome, but if you have a field such as firstName	 on more than one endpoint. You end up having different explanations in the docs for this, and they become very inconsistent. And again now this code pollutes the unit test code being written. 

[Spring Auto REST Docs](https://github.com/ScaCap/spring-auto-restdocs)
> is an extension or Spring REST Docs to help you write even less - both code and documentation.
> it's able to extract JSON field names and types from request and response classes.
as well as recognising common bean validation annotations such as @NotNull, @Size
and 
> Instead of using custom annotations like Swagger or polluting tests to describe what a field represents
they use already standard JavaDocs on fields and functions

__BUT__ ... even with the Spring REST/AutoRest Doc, differentiator's. We are still missing the "playground" area SwaggerUI supplies. 

__I therefore conclude that a combination of the two should actually be used, rather than assuming they compete with each other.__

Further thoughts
- There are now extensions to Swagger/SwaggerUI that also read comments from the JavaDoc rather than the special annotations such as [swagger-doclet](https://github.com/conorroche/swagger-doclet) and [springfox-javadoc](https://github.com/springfox/springfox-javadoc) neither are fully fledged and don't pick up all JavaDoc yet. 
- The other alternative is to do contract first design. Which I have attempted, but never quite succeeded. I assume this is because like [Carlos Barragan](https://blog.novatec-gmbh.de/the-problems-with-swagger/) says :
> If you happen to do contract first, I can tell you it is not a pleasant experience to write the YAML yourself for non-trivial APIs even with the swagger editor.

As an aside... 
- in the .Net world, there is [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle) for SwaggerUI and swagger.json generation. Which allows you to include "XML Comments", which is the equivalent of JavaDoc, so they don't seem to have this issue for attribute descriptions.

References
====
- [https://spring.io/projects/spring-restdocs](https://spring.io/projects/spring-restdocs) 
- [https://dzone.com/articles/introducing-spring-auto-rest-docs](https://dzone.com/articles/introducing-spring-auto-rest-docs)
- [https://blog.novatec-gmbh.de/the-problems-with-swagger/](https://blog.novatec-gmbh.de/the-problems-with-swagger/)
- [https://dzone.com/articles/swagger-great](https://dzone.com/articles/swagger-great)
- [https://springframework.guru/spring-boot-restful-api-documentation-with-swagger-2/](https://springframework.guru/spring-boot-restful-api-documentation-with-swagger-2/)
- [https://dzone.com/articles/spring-rest-docs-versus-springfox-swagger-for-api](https://dzone.com/articles/spring-rest-docs-versus-springfox-swagger-for-api)
- [https://github.com/domaindrivendev/Swashbuckle](https://github.com/domaindrivendev/Swashbuckle)