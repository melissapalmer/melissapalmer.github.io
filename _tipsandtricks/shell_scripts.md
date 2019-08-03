---
layout: post
comments: true
title: "Shell Scripts"
categories: tips and tricks
tags: 
- Shell Scripts
description: Shell Scripts
published: true
---

* [Find process running on port](#find_process_running_on_port)
* [Create Desktop Launcher](#create_desktop_launcher)  

<a name="find_process_running_on_port"/>
**Find process running on port 9981**     `sudo netstat -ltnp | grep -w ':9981'`
*NOTE:* to run the above you do need `net-tools` installed you can use `sudo apt-get install net-tools` to install 

