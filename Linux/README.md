# Linux Notes

#### Quick reference

## Assorted Commands

**Arandr** - application to configure multiple screens on debian based systems(and others?)

## SCP command

* SCP copy a folder to remote server

..* scp -r foo your_username@remotehost.edu:/some/remote/directory/bar

* SCP copy file with port

..* scp -P 2264 foobar.txt your_username@remotehost.edu:/some/remote/directory

## SendEmail

* website:

> http://caspian.dotconf.net/menu/Software/SendEmail/

`sendEmail -o tls=no -f <EMAIL FROM> -t <EMAIL TO> -s <SMTP SERVER> -xu <SERVER LOGIN> -xp <PASSWORD> -u <SUBJECT> -m <MESSAGE TO SEND>`
