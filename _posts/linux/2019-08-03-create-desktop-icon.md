---
layout: post
comments: true
title:  "Create a desktop shortcut"
date: 2019-08-03 06:45:09 -0200
categories: linux
tags: 
- Tips
description: Create a desktop shortcut
---

Install gnome-desktop-item-edit using the command:
`sudo apt  install --no-install-recommends gnome-panel`

To create a new desktop shortcut launcher:
`gnome-desktop-item-edit ~/Desktop/ --create-new`

This will open the Create Launcher dialog, populate as you would like the short cut details to be and click OK. 
![Create Launcher dialog](/assets/images/linux/create_shortcut/1.png)

This will create a .desktop file will look something like below
```
#!/usr/bin/env xdg-open
[Desktop Entry]
Version=1.0
Type=Application
Terminal=false
Icon[en_ZA]=gnome-panel-launcher
Name[en_ZA]=SpringSTS
Exec=/home/melissa/apps/spring-tool-suite-4-4.3.1.RELEASE-e4.12.0-linux.gtk.x86_64/sts-4.3.1.RELEASE/SpringToolSuite4
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
Icon[en_ZA]=/home/melissap/apps/spring-tool-suite-4-4.2.0.RELEASE-e4.11.0-linux.gtk.x86_64/Core_Spring_Icon.png.png
Name[en_ZA]=SpringSTS
Exec=/home/melissa/apps/spring-tool-suite-4-4.3.1.RELEASE-e4.12.0-linux.gtk.x86_64/sts-4.3.1.RELEASE/SpringToolSuite4
Comment[en_ZA]=SpringSTS
Name=SpringSTS
Comment=SpringSTS
Icon=/home/melissap/apps/spring-tool-suite-4-4.2.0.RELEASE-e4.11.0-linux.gtk.x86_64/Core_Spring_Icon.png.png
```

Now move the .desktop file  to the applications folder, thus making it visible to the Applications lens:
`sudo mv ~/Desktop/SpringSTS.desktop /usr/share/applications/`



References
====
- [https://linuxconfig.org/how-to-create-desktop-shortcut-launcher-on-ubuntu-18-10-cosmic-cuttlefish-linux](https://linuxconfig.org/how-to-create-desktop-shortcut-launcher-on-ubuntu-18-10-cosmic-cuttlefish-linux)
