---
layout: post
title: "Docker for Unix on Windows with Vagrant"
date: 2018-08-04 17:45:09 -0700
categories: testing
tags: 
- Postman
- Docker
description: Docker for Unix on Windows with Vagrant
published: true
---

Newman is a command-line collection runner for Postman.



docker run --name=postman-newman -v "$(pwd)":/etc/postman-newman postman/newman_ubuntu1404 run /etc/postman-newman/GOOGLE.postman_collection.json

docker run --name=postman-newman -v "$(pwd)":/etc/postman-newman postman/newman_ubuntu1404 run /etc/postman-newman/GOOGLE.postman_collection.json --environment=/etc/postman-newman/GOOGLE_ENV.postman_environment.json

docker run --name=postman-newman -v "$(pwd)":/etc/postman-newman postman/newman_ubuntu1404 run /etc/postman-newman/GOOGLE.postman_collection.json --environment=/etc/postman-newman/GOOGLE_ENV.postman_environment.json --reporters cli,html --reporter-html-export /etc/postman-newman/outputfile.html
newman






```
vagrant@vagrant-ubuntu-trusty-64:~$ docker run --name=postman-newman -v "$(pwd)":/etc/postman-newman postman/newman_ubuntu1404 run /etc/postman-newman/GOOGLE.postman_collection.json
newman

GOOGLE

→ ViewHomePage
  GET https://www.google.com/ [200 OK, 12.55KB, 1264ms]
  ✓  Status code is 200
  ✓  Response time is less than 5000ms

┌─────────────────────────┬──────────┬──────────┐
│                         │ executed │   failed │
├─────────────────────────┼──────────┼──────────┤
│              iterations │        1 │        0 │
├─────────────────────────┼──────────┼──────────┤
│                requests │        1 │        0 │
├─────────────────────────┼──────────┼──────────┤
│            test-scripts │        1 │        0 │
├─────────────────────────┼──────────┼──────────┤
│      prerequest-scripts │        0 │        0 │
├─────────────────────────┼──────────┼──────────┤
│              assertions │        2 │        0 │
├─────────────────────────┴──────────┴──────────┤
│ total run duration: 1900ms                    │
├───────────────────────────────────────────────┤
│ total data received: 11.85KB (approx)         │
├───────────────────────────────────────────────┤
│ average response time: 1264ms                 │
└───────────────────────────────────────────────┘
vagrant@vagrant-ubuntu-trusty-64:~$
```

References
===

http://blog.getpostman.com/2015/07/30/official-docker-image-for-newman/
https://github.com/postmanlabs/newman/tree/develop/docker
https://www.getpostman.com/docs/v6/postman/collection_runs/newman_with_docker





https://www.getpostman.com/docs/v6/api_testing_and_collection_runner/newman_intro
https://dzone.com/articles/how-to-get-command-line-integration-with-newman-in?fromrel=true
https://dzone.com/articles/testing-apis-using-postman



