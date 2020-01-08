---
layout: post
comments: true
title: "Postgres with Docker Ticks and Tricks"
categories: tips and tricks
tags: 
- Docker
- Postgres
description: Postgres with Docker
published: true
---

# Log into Postgres on a Docker container
`docker exec -it <container_name> psql -U <database_username>`

# Log into a Postgres DB on a Docker container
`docker exec -it <container_name> psql -U <database_username> -a <database_name>`



<a name="references"/>
# References
- [Tricks for Postgres and Docker that will make your life easier](https://towardsdatascience.com/tricks-for-postgres-and-docker-that-will-make-your-life-easier-fc7bfcba5082)
