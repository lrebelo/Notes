# elementary OS 0.4

### Adding a new application to the application menu..

go to `cd /usr/share/applications/`

&

`sudo touch NAMEofAPPLICATION.desktop`

then past in it:

```
[Desktop Entry]
Encoding=UTF-8
Name=NAME of APPLICATION
Comment=Android IDE
Exec=/usr/local/bin/APP
Icon=APP_ICON
Terminal=false
Type=Application
Categories=GNOME;Application;Development;
StartupNotify=true
```

save and press on the menu to see changes...
If nothing happens its possibel its just not on the right Categorie OR the path to the bin is incorrect.

## Scratch

### Scratch keeps crashing

`gsettings reset-recursively org.pantheon.scratch.settings`
`gsettings reset-recursively org.pantheon.scratch.saved-state`

execute and hope! :)

## BOOTing on old intel based macs (tested on gen3)

Edit `nano /etc/default/grub`

add `video=SVIDEO-1:d` so that it looks like:

`GRUB_CMDLINE_LINUX_DEFAULT="quiet splash video=SVIDEO-1:d"`

`update-grub` & reboot

(all as root... )
