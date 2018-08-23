---
layout: post
comments: true
title:  "Java Code metrics with JaCoCo, TravisCi and SonarCloud"
date: 2018-08-09 09:45:09 -0700
categories: testing
tags: 
- SonarQube
- TravisCi
description: Java Code metrics with JaCoCo, TravisCi and SonarCloud
---

# Overview

> Why analyze source code? Simple put: to ensure quality, reliability, and maintainability over the life-span of the project; a poorly written codebase is always more expensive to maintain.

In this post we look at how you can automate static code analysis with JaCoCo, TravisCi and SonarCloud. 

# Description of JaCoCo, TravisCi and SonarCloud.

- [SonarQube](https://www.sonarqube.org/about/) is an open source platform that automates static code analysis to detect bugs, code smells and security vulnerabilities on 20+ programming languages 
- [SonarCloud](https://sonarcloud.io/about) is the cloud-hosted version of SonaQube server.
- [JaCoCo](https://www.jacoco.org/jacoco/) is a free Java Code Coverage library. Code Coverage is a measure used to describe the degree to which the source code of a program is executed when test are run against it. 
- [Travis CI](https://about.travis-ci.com/) is a hosted, distributed continuous integration service used to build and test software projects hosted at GitHub.

# Steps

1. Create SonarCloud account
2. Run Maven 
3. Integration with Travis CI
4. Add Token to GitHub setting for repo.

## Create SonarCloud account

Go https://sonarcloud.io and link your GitHub repo with SonarCloud. Choose a key

## Run Maven with jacoco-maven-plugin and sonar:sonar

Passing the following parameters sonar.organisation, sonar.host.url and sonar.login, as below:  

`mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent package sonar:sonar -Dsonar.organization=YOUR_ORG -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=YOUR_KEY`

this will get Maven to run the JaCoCo Maven plugin (ie: org.jacoco:jacoco-maven-plugin) that will will generate code coverage reports off your code base, on the unit tests included.  
A new file will be generated to the target directory, see output lines similar to those below: 

```
[INFO] --- jacoco-maven-plugin:0.8.1:prepare-agent (default-cli) @ spring-boot-github-sonarqube ---
[INFO] argLine set to -javaagent:C:\\Users\\melissa.palmer\\.m2\\repository\\org\\jacoco\\org.jacoco.agent\\0.8.1\\org.jacoco.agent-0.8.1-runtime.jar=destfile=C:\\work\\projects\\opensource\\github.com.melissapalmer\\spring-boot-github-sonarqube\\target\\jacoco.exec
[INFO]
```

However, the `jacoco.exec` file format is not readable by humans, but it can be sent to SonarQube for more analysis. That is what the sonar related paramaters we used in the command above do. In the logs you see lines simlar to those below, where Maven is sending the JaCoCo file to SonarCloud for further analysis by SonarQube.   

```
[INFO] --- sonar-maven-plugin:3.4.1.1168:sonar (default-cli) @ spring-boot-github-sonarqube ---
[INFO] User cache: C:\Users\melissa.palmer\.sonar\cache
[INFO] SonarQube version: 7.3.0
[INFO] Default locale: "en_ZA", source code encoding: "UTF-8"
...
...
[INFO] ANALYSIS SUCCESSFUL, you can browse https://sonarcloud.io/dashboard?id=com.example%3Aspring-boot-github-sonarqube
[INFO] Note that you will be able to access the updated dashboard once the server has processed the submitted analysis report
[INFO] More about the report processing at https://sonarcloud.io/api/ce/task?id=AWUeSdusmy2lpaU5TWSh
[INFO] Task total time: 38.757 s
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:01 min
[INFO] Finished at: 2018-08-09T12:44:42+02:00
[INFO] Final Memory: 72M/583M
[INFO] ------------------------------------------------------------------------
```

Have a look at [![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=com.example%3Aspring-boot-github-sonarqube&metric=alert_status)](https://sonarcloud.io/dashboard?id=com.example%3Aspring-boot-github-sonarqube) for the code analysis report of this project on SonarCloud. 

## Integration with Travis CI

So far we've only done this locally. Let's integrate our build with Travis CI, so that we can run this analysis on all commits to the code base on GitHub. To do this you need to add the Travis CI config file to your repo. 

First go to your Travis CI account and create an environment variable for SONAR_TOKEN to hold your SonarCloud secret. (As you don't want that seen in GitHub by others). 
Then add a file called `.travis.yml` to your repo, with the following setup: 

```
language: java
dist: trusty
sudo: required

before_install:
  - chmod +x mvnw

addons:
  sonarcloud:
    organization: "sonarcloud-org-id"
    token:
      secure: $SONAR_TOKEN # Set in the settings page of your repository, as a secure variable

jdk:
  - oraclejdk8

script:
  # the following command line builds the project, runs the tests with coverage and then execute the SonarCloud analysis
  - mvn clean org.jacoco:jacoco-maven-plugin:prepare-agent package sonar:sonar

cache:
  directories:
    - '$HOME/.m2/repository'
    - '$HOME/.sonar/cache'
```

Next time you push to your GitHub repo, the Travis CI build will kick in and it will run: 
 
- a Maven build 
- including running of the jacoco-maven-plugin to generate the jacoco.exec
- and push the jacoco.exec to SonarCloud for further analysis
- you can then go to SonarCloud and view the results of all this analysis agaisnt your code base. 


## Adding badges to the README

You can then also add badges to your GitHub repo to show the build status of your project, and the code analysis status such as those below:

[![Build status](https://travis-ci.org/melissapalmer/spring-boot-github-sonarqube.svg?branch=master)](https://travis-ci.org/melissapalmer/spring-boot-github-sonarqube) [![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=com.example%3Aspring-boot-github-sonarqube&metric=alert_status)](https://sonarcloud.io/dashboard?id=com.example%3Aspring-boot-github-sonarqube)

- For GitHub add: 

	`[![Build status](https://travis-ci.org/melissapalmer/spring-boot-github-sonarqube.svg?branch=master)](https://travis-ci.org/melissapalmer/spring-boot-github-sonarqube)`
- For SonarCloud add: 

	`[![Quality Gate](https://sonarcloud.io/api/project_badges/measure?project=com.example%3Aspring-boot-github-sonarqube&metric=alert_status)](https://sonarcloud.io/dashboard?id=com.example%3Aspring-boot-github-sonarqube)`

# References
- [https://thepracticaldeveloper.com/2016/02/06/test-coverage-analysis-for-your-spring-boot-app/](https://thepracticaldeveloper.com/2016/02/06/test-coverage-analysis-for-your-spring-boot-app/)
- [https://docs.travis-ci.com/user/sonarcloud/](https://docs.travis-ci.com/user/sonarcloud/)
- [https://github.com/SonarSource/sq-com_example_java-maven-travis](https://github.com/SonarSource/sq-com_example_java-maven-travis)
- [https://www.baeldung.com/sonar-qube](https://www.baeldung.com/sonar-qube)

