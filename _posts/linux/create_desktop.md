f you want a .desktop file for easy launching in Ubuntu (tested on 18.04) Rather than manually create the .desktop file there is a useful tool gnome tool:

sudo apt-get install --no-install-recommends gnome-panel
Create a .desktop file on my ~/Desktop and afterwards moved it to the applications folder, thus making pgmodeler visible to the Applications lens

gnome-desktop-item-edit ~/Desktop/ --create-new
sudo mv pgmodeler.desktop /usr/share/applications/
and it should result in a .desktop file like this:

#!/usr/bin/env xdg-open
[Desktop Entry]
Version=1.0
Type=Application
Terminal=false
Icon[en_US]=/opt/pgmodeler/conf/pgmodeler_logo.png
Name[en_US]=pgmodeler
Exec=/opt/pgmodeler/start-pgmodeler.sh
Comment[en_US]=pgmodeler 0.9.2
Name=pgmodeler
Comment=pgmodeler 0.9.2
Icon=/opt/pgmodeler/conf/pgmodeler_logo.png
referenced source https://github.com/pgmodeler/pgmodeler/issues/674
https://github.com/GrindrodBank/canonical-schema/blob/d6acc9efa546e8b8dfc674724a690e798f840e24/README.md