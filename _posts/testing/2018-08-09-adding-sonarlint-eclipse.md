---
layout: post
title:  "Java Code metrics with SonarLint in Eclipse"
date: 2018-08-09 10:45:09 -0700
categories: testing
tags: 
- SonarQube
- SonarLint
description: Java Code metrics with SonarLint in Eclipse
---

# Overview

[SonarQube](https://www.sonarqube.org/about/) is an open source platform that automates static code analysis to detect bugs, code smells and security vulnerabilities on 20+ programming languages. 

Usually a company will setup SonarQube server or user SonarCloud integrated with a CI server to fail the build when quality gates are not met. It's always better to fix things earlier than later. [SonarLint](https://www.sonarlint.org/) allows you to
> Fix issues before they exist

[SonarLint](https://www.sonarlint.org/) is an IDE extension that helps you detect and fix quality issues as you write code. 

In this post I describe how to include SonarLint in your Eclipse IDE. 

# Steps 

1. Include Eclipse plugin for SonarLint (by adding a Software Update site) 
2. Enable SonarLint on your project
3. Run SonarQube analysis on the project
4. Checkout different Eclipse views for SonarLint/SonarQube analisys results

## Include Eclipse plugin for SonarLint (by adding a Software Update site) 

In Eclipse go to : 

`Help --> Install New Software --> add URL https://eclipse-uc.sonarlint.org`

![Eclipse Software Site for SonarQube](/assets/images/testing/EclipseSoftwareSite_SonarQube.JPG)

Once you have installed the plugin, you'll need to restart Eclipse as usual. THEN 

## Enable SonarLint on your project

by right clicking on your project in the Package Explorer and bind project to a server as below: 

![Eclipse Enable SonarQube on Project](/assets/images/testing/EclipseEnable_SonarQube.JPG)

If its the first time you doing this, you will need to Connect a SonarQube server. You will be prompted for your SonarCloud secret, and organization name. 
Once your SonarQube server is added, and you've bind the project. Eclipse will run a Update data from SonarQube. 

You can then right click on the project again in the Package Explorer and chose SonarLint --> Analyze to run that code analysis. 
To see the results of this analysis you will need to add a couple Views to your Eclipse perspective by going to: 

Show Views
- SonarLint --> SonarLint On-The-Fly, SonarLint Rule Description 

I found the above two views the most useful, there are others you can add and decide for yourself. 

See below how the On-The-Fly view, highlights in the code issues, telling you which SonarQube rule this relates to. To get more infomation about the Rule use the SonarLint Rule Description view which will give you a full description of the Rule for issue you looking at. 

![Eclipse Enable SonarQube on Project](/assets/images/testing/EclipseViews_SonarQube.JPG)

# References
- [https://www.sonarlint.org/](https://www.sonarlint.org/)
- [https://marketplace.eclipse.org/content/sonarlint](https://marketplace.eclipse.org/content/sonarlint)
- [https://www.sonarlint.org/eclipse/](https://www.sonarlint.org/eclipse/)

