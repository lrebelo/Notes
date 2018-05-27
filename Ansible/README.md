# Ansible Notes

Install on all machines

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

## Ansible Playbook(s)


Playbooks are divided by sections (wtw: in _Ansible Galaxy_ they are not) `hosts`, `vars`, `tasks`, `handlers`

```
---
- hosts: clients
  remote_user: ansible

  vars:
  - client_number: 1

  tasks:
  - name: Name Of task
    <add config as per ansible module>

  handlers:
  - name: start pm2 app
    command: ''
    become: yes
    become_user: root

```

To execute a playbook we do `ansible-playbook -i <playbookname>.yml`

To execute a playbook with external given variables we do `ansible-playbook -i hosts <playbookname>.yml -e "variwant=VALUEONE" -e "secondvariwant=VALUETWO`


_work in progress..._

***

##### References..

```
https://serversforhackers.com/c/an-ansible2-tutorial
```
