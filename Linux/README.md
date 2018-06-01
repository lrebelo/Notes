# Linux Notes

#### Quick reference

---
## Assorted Commands

`Arandr` - application to configure multiple screens on debian based systems(and others?)

---
## SCP command

* SCP copy a folder to remote server

    * `scp -r foo your_username@remotehost.edu:/some/remote/directory/bar`

* SCP copy file with port

    * `scp -P 2264 foobar.txt your_username@remotehost.edu:/some/remote/directory`

## SendEmail

* website:

> http://caspian.dotconf.net/menu/Software/SendEmail/

`sendEmail -o tls=no -f <EMAIL FROM> -t <EMAIL TO> -s <SMTP SERVER> -xu <SERVER LOGIN> -xp <PASSWORD> -u <SUBJECT> -m <MESSAGE TO SEND>`

## Getting IP address only for an interface

* I was working on an Android app and had the need to get the ipaddress of an interface and came up with the following grep/awk command..

`ifconfig eth0 | grep 'inet addr:' | awk -F ':' '{print $2}' | awk -F ' ' '{print $1}'`

## Adding a user the generic linux way

`useradd`

* `-m` create home
* `-G` assign extra groups

example:
`useradd -m -G www-data,video,users,tty username`

## Blocking tty jumping while in X

* Append the lines bellow to `/etc/X11/xorg.conf` it will block any attempt at jumping

```
Section "ServerFlags"
    Option "DontVTSwitch" "true"
EndSection
```

## Quick and easy way to change a user password from a shell script

`echo "username:password" | chpasswd`

## Lessons from `echo`

* `-e` will allow me to `echo` any escaped chars (`echo -e "Hello\\nWorld" == Hello \n World`)

* when needing to `echo` out a shell script the `#!/bin/bash` will cause issues due to the `!` so we can use the following command before said `echo`
    * `set +o histexpand`

## `BASH` configuration(s)

* When wanting to start X as soon as `bash` starts we can use the following lines by appending them to the `.bash_profile` file.
```
if [ -z "$DISPLAY" ] && [ -n "$XDG_VTNR" ] && [ "$XDG_VTNR" -eq 1 ]; then
  exec startx
fi
```

## `xinitrc`

* nothing much to be said except that remember that we can write entire shell script loops into this file..

## `xrandr`

* allows us to change the resolution of a screen even if the console we are using is not within the same X session.

`xrandr -d :0 -s 1920x1080`


```
usage: xrandr [options]
  where options are:
  -display <display> or -d <display>
  -help
  -o <normal,inverted,left,right,0,1,2,3>
            or --orientation <normal,inverted,left,right,0,1,2,3>
  -q        or --query
  -s <size>/<width>x<height> or --size <size>/<width>x<height>
  -r <rate> or --rate <rate>
  -v        or --version
  -x        (reflect in x)
  -y        (reflect in y)
  --screen <screen>
  --verbose
```

### DisplayLink adapters

I got myself a USB2.0 DisplayLink adapter that works +/-
However by default it only does 1024x7268..
`xrandr` can solve that thought!
`xrandr --addmode DVI-I-1-1 1366x768`

My device came up as `DVI-I-1-1` when doing an `xrandr`.
This allowed me to set the screen with the resolution I wanted in `arandr`.

## Battery information

`upower -i /org/freedesktop/UPower/devices/battery_BAT0`

## Sound control bash

`pacmd list-cards | grep output\:` - give us a list of all the available audio outputs.

`pactl set-card-profile 0 output:hdmi-stereo+input:analog-stereo` - set audio output from both the analog and hdmi outputs. For the right output see the command that came before.

`speaker-test` - makes an ungodly noise from the command line.. helpful for testing the sound.

## Linux File System Tools/Commands


`rsync -autzv _SOURCE_ _DESTINATION_` - recursive sync/copy tool(options are for: a archive, u update, t times, z compress, v verbose)

`//192.168.1.256/share  /media/share  cifs  guest,uid=1000,iocharset=utf8  0  0` - mount as a guest

`//192.168.1.256/share       /media/share  cifs username=USERNAME, password=PASSWORD, iocharset=utf8, sec=ntlm 0 0` - mounting samba on boot

To add a space on in the path replace the space `\040`

 Install `sudo apt-get install cifs-utils` to be able to use samba at boot time.

`df -h` - report file system disk space usage(h for human, m for megs)

`blkid` - as root to find the UUID of local drives


## Nodejs LTS (8.x) install for debian Linux 8


**I normally do this throught as root rather than sudo for spead sake when configuring on a server not a dev machine**

`curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -`

`sudo apt-get install nodejs_`

`apt-get install build-essential`

**IF using ElementaryOS Loki**

- go: `sudo nano /etc/apt/sources.list.d/nodesource.list` and change the distro name to xenial as the rep has no hook for loki


**If upgrading..**

`apt-get purge nodejs npm`


## MongoDB install for debian Linux 8


**I normally do this through root rather than sudo for speed sake**

`echo "deb http://repo.mongodb.org/apt/debian jessie/mongodb-org/3.4 main" | tee /etc/apt/sources.list.d/mongodb-org-3.4.list`

`apt-get update`

`apt-get install mongodb-org`

`service mongod start`

`systemctl enable mongod` - in debian and sometimes ubuntu the service does not get set to auto start on boot


### Setup /etc/mongodb.conf file

`bindIp: 0.0.0.0` - setup to access from all IPs (or restrict for specific ranges or IPs)

#### Security

`db.createUser({user: "admin", pwd: "password", roles: [ { role: "userAdminAnyDatabase", db: "admin" }]} )`

_security:_
_authorization: "enabled"_  - Only add this after creating the admin user


### Assorted Commands

`use db` - use an existing db, if it doesn't it will create it uong data entry.

`show dbs` - show list of databases.

`show collections` - show list of collections.
