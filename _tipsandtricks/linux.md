---
layout: post
comments: true
title: "Linux Ticks and Tricks"
categories: tips and tricks
tags: 
- Linux
description: Linux
published: true
---

* [Find process running on port](#find_process_running_on_port)
* [Create Desktop Launcher](#create_desktop_launcher)  

<a name="find_process_running_on_port"/>
**Find process running on port 9981**     `sudo netstat -ltnp | grep -w ':9981'`
*NOTE:* to run the above you do need `net-tools` installed you can use `sudo apt-get install net-tools` to install 

<a name="create_desktop_launcher"/>
**Create Desktop Launcher**
Install `gnome-desktop-item-edit` command:
`sudo apt  install --no-install-recommends gnome-panel`
To create a new desktop shortcut launcher :
`gnome-desktop-item-edit ~/Desktop/ --create-new`

Using a text editor the .desktop file will look somethign like below
```
#!/usr/bin/env xdg-open
[Desktop Entry]
Version=1.0
Type=Application
Terminal=false
Icon[en_ZA]=gnome-panel-launcher
Name[en_ZA]=SpringSTS
Exec=/home/melissap/apps/spring-tool-suite-4-4.2.0.RELEASE-e4.11.0-linux.gtk.x86_64/sts-4.2.0.RELEASE/SpringToolSuite4
Comment[en_ZA]=SpringSTS
Name=SpringSTS
Comment=SpringSTS
Icon=gnome-panel-launcher
```

You can edit this to use you own icon, eg: 
```
#!/usr/bin/env xdg-open
[Desktop Entry]
Version=1.0
Type=Application
Terminal=false
Icon[en_ZA]=/home/melissap/apps/spring-tool-suite-4-4.2.0.RELEASE-e4.11.0-linux.gtk.x86_64/Core_Spring_Icon_6ee96dfa-445b-4745-b62d-da2007f81e54_1024x1024.png
Name[en_ZA]=SpringSTS
Exec=/home/melissap/apps/spring-tool-suite-4-4.2.0.RELEASE-e4.11.0-linux.gtk.x86_64/sts-4.2.0.RELEASE/SpringToolSuite4
Comment[en_ZA]=SpringSTS
Name=SpringSTS
Comment=SpringSTS
Icon=/home/melissap/apps/spring-tool-suite-4-4.2.0.RELEASE-e4.11.0-linux.gtk.x86_64/Core_Spring_Icon_6ee96dfa-445b-4745-b62d-da2007f81e54_1024x1024.png
```
Now move the .desktop file  to the applications folder, thus making it visible to the Applications lens:
`sudo mv SpringSTS.desktop /usr/share/applications/`

References
# [https://linuxconfig.org/how-to-create-desktop-shortcut-launcher-on-ubuntu-18-10-cosmic-cuttlefish-linux](https://linuxconfig.org/how-to-create-desktop-shortcut-launcher-on-ubuntu-18-10-cosmic-cuttlefish-linux)
