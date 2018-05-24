# Ansible Notes

Install on all mahines

`apt-get install -y ansible`

used `ssh-keygen` to generate keys and `ssh-copy-id` to send keys to host machines.

create a `hosts` file or modify the oen in `/etc/ansible/hosts` and add your own servers.
```
[testservers]
192.168.1.2
192.168.1.3
192.168.1.4
```

to test connection do `ansible all -m ping`

_work in progress..._

***

##### References..

```
https://serversforhackers.com/c/an-ansible2-tutorial
```
