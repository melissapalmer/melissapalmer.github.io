---
layout: post
title:  "Swagger Codegen 2 Java Classes with Maven"
date: 2018-07-26 17:45:09 -0700
categories: documentation
tags: 
- Swagger Codegen
- Maven
- Spring Boot
description: Swagger Codegen 2 Java Classes with Maven
---

Swagger Codegen can be used to generate stubs/model classes in 'popular languages, like Java, Scala, and Ruby'. As such we can use it to generate the basis for calling other APIs in a polygot environment. 

[swagger-codegen-maven-plugin](https://github.com/swagger-api/swagger-codegen/tree/master/modules/swagger-codegen-maven-plugin) is a 
> A maven build plugin which allows the codegen project to be triggered for generating clients, etc. during the build process.
for a Java project.

To generate the code from the swagger.json files it is as simple as including the swagger.json file for the API you want to consume into your project under
`/src/main/resources/`

Adding the swagger-codegen-maven-plugin to your `build->plugins` section (default phase is generate-sources phase) with matching configuration parameters for your project. The documentation for this plugin is at: 

[https://github.com/swagger-api/swagger-codegen/tree/master/modules/swagger-codegen-maven-plugin](https://github.com/swagger-api/swagger-codegen/tree/master/modules/swagger-codegen-maven-plugin)

However to then ensure that this code compiles, there are a couple of other dependencies you need to add to your pom.xml file. These include: 
- swagger-annotations
- org.threeten
- com.google.code.gson

An example of the pom.xml is: 
```xml
<project>
	...
	<properties>
		...
		<swagger-annotations-version>1.5.8</swagger-annotations-version>
        <threetenbp-version>1.3.5</threetenbp-version>
	</properties>

	<dependencies>
		...
		<!-- Dependencies to enable compiling of the code generated from swagger.json - Petstore API-->
		<dependency>
			<groupId>io.swagger</groupId>
			<artifactId>swagger-annotations</artifactId>
			<version>${swagger-annotations-version}</version>
		</dependency>
		<dependency>
			<groupId>org.threeten</groupId>
			<artifactId>threetenbp</artifactId>
			<version>${threetenbp-version}</version>
		</dependency>
		<dependency>
	    	<groupId>com.google.code.gson</groupId>
	    	<artifactId>gson</artifactId>
		</dependency>
		...
	</dependencies>

	<build>
		<plugins>
			...
			<plugin>
                <groupId>io.swagger</groupId>
                <artifactId>swagger-codegen-maven-plugin</artifactId>
                <version>2.3.1</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>generate</goal>
                        </goals>
                        <configuration>
                            <inputSpec>${project.basedir}/src/main/resources/petstore-swagger.json</inputSpec>
                            <language>java</language>
                            <output>${project.basedir}</output>

                            <apiPackage>com.example.demo.petstore.api</apiPackage>
                            <modelPackage>com.example.demo.petstore.api.model</modelPackage>
                            <generateApis>false</generateApis>
                            <generateApiTests>false</generateApiTests>
                            <generateApiDocumentation>false</generateApiDocumentation>
                            <generateSupportingFiles>false</generateSupportingFiles>
                            <generateModelDocumentation>false</generateModelDocumentation>

                            <configOptions>
                                <sourceFolder>src/main/gensrc</sourceFolder>
                            </configOptions>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
		</plugins>
	</build>
	...
</project>
```

Source code for this is example is at: [https://github.com/melissapalmer/swagger-codegen-java-models](https://github.com/melissapalmer/swagger-codegen-java-models)

References
====
- [https://swagger.io/tools/swagger-codegen/](https://swagger.io/tools/swagger-codegen/)
- [https://github.com/swagger-api/swagger-codegen/tree/master/modules/swagger-codegen-maven-plugin](https://github.com/swagger-api/swagger-codegen/tree/master/modules/swagger-codegen-maven-plugin) 
