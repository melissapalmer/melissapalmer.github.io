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
* [Count all files recursively](#count_all_files_recursively)  

<a name="find_process_running_on_port"/>
**Find process running on port 9981**     
`sudo netstat -ltnp | grep -w ':9981'`
*NOTE:* to run the above you do need `net-tools` installed you can use `sudo apt-get install net-tools` to install 

<a name="count_all_files_recursively"/>
**Count all the files recursively** `find -maxdepth 1 -type d | while read -r dir; do printf "%s:\t" "$dir"; find "$dir" -type f | wc -l; done`
_Reference_ https://unix.stackexchange.com/questions/4105/how-do-i-count-all-the-files-recursively-through-directories

