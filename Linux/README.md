# Linux Notes

#### Quick reference

## Assorted Commands

`Arandr` - application to configure multiple screens on debian based systems(and others?)

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

* I was working on an Android app and had the need to get the ipaddress of an interface and came up with the follwing grep/awk command..

`ifconfig eth0 | grep 'inet addr:' | awk -F ':' '{print $2}' | awk -F ' ' '{print $1}'`
