# Nginx Notes

#### Quick reference


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

    * See the list of apps and 'select' the appropriate one `sudo ufw app list`

    * After choosing enable the right one.. `sudo ufw allow 'Nginx HTTP'`

    * Make sure its active.. `sudo ufw status`

## Configure Nginx

* Main configuration in `/etc/nginx/nginx.conf`

#### Reverse Proxy config

* Example:

```
proxy_set_header HOST $host;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_Header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

location /match/here {
    proxy_pass http://example.com/new/prefix;
}

location /different/match {
    proxy_pass http://example.com;
}
```



---

##### References

`http://nginx.org/en/docs/stream/ngx_stream_proxy_module.html`

`https://www.nginx.com/resources/admin-guide/reverse-proxy/`




