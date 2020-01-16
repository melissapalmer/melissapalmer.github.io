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
```
docker exec -it <container_name> psql -U <database_username>
``

# Log into a Postgres DB on a Docker container
```
docker exec -it <container_name> psql -U <database_username> -a <database_name>
```


# Backing up Your Data
```
docker exec -it <container_name> pg_dump -U <user_name> --column-inserts --data-only <db_name> > backup.sql
```

eg: 
```
docker exec -it ocs-postgresql pg_dump -U postgres --column-inserts --data-only ocs > backup.sql
```

# Restore data from sql files
```
docker cp ./backup.sql <container_name>:/data.sql
docker exec -it <container_name> psql -U <user_name> -a <db_name> -f /data.sql
```

eg:
```
docker cp ./backup.sql ocs-postgresql:/data.sql
docker exec -it ocs-postgresql psql -U postgres -a ocs -f /data.sql
```

<a name="references"/>
# References
- [Tricks for Postgres and Docker that will make your life easier](https://towardsdatascience.com/tricks-for-postgres-and-docker-that-will-make-your-life-easier-fc7bfcba5082)
