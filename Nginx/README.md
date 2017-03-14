# Nginx Notes

####Quick reference


## Instalation for debian8

* Install with apt-get

```
apt-get install nginx
``` 

## Instalation for ubuntu16.04

* Install with apt-get

```
sudo apt-get install nginx
``` 

* Adjust the firewall

..* See the list of apps and 'select' the appropriate one.

```
sudo ufw app list
```

..* After choosing enable the right one..

```
sudo ufw allow 'Nginx HTTP'
```

..* Make sure its active..

```
sudo ufw status
```

## Configure Nginx

* Main configuration in `/etc/nginx/nginx.conf`



