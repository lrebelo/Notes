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

    * What we would put in the /etc/nginx/sites/enabled/<domain>

```

server {
    listen 80;
    listen [::]:80;

    server_name domain.com;

    location /location2/{
        proxy_pass http://xxx.xxx.xxx.xxx:3000/;
    }
    location /location1/{
        proxy_pass http://xxx.xxx.xxx.xxx:3001/;
        proxy_set_header HOST $host;
    }
}

```

#### Add HTTPS (SSL) to the configuration.

Create a directory where to store the certificates:
`mkdir /etc/nginx/ssl`

Generate the certificate by doing:
`openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/nginx/ssl/nginx.key -out /etc/nginx/ssl/nginx.crt`

We can create multiple certificates for diferent hostnames.

Then add the following basic configation to nginx to set your link as https.

```
server {

    listen 443 ssl;

    server_name <FQDN>;
    ssl_certificate /etc/nginx/ssl/nginx.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx.key;

    location /{
        proxy_pass http://xxx.xxx.xxx.xxx:3002/;
    }

}

```

#### Auto redirect all _http_ request to _https_

With the code bellow all _http_ subdomains get redirected to _https_

```
server {

    listen 80;

    server_name *.<example.com>;

    return 301 https://$host$request_uri;
}

```


---

##### References

`http://nginx.org/en/docs/stream/ngx_stream_proxy_module.html`

`https://www.nginx.com/resources/admin-guide/reverse-proxy/`

`https://www.digitalocean.com/community/tutorials/how-to-create-an-ssl-certificate-on-nginx-for-ubuntu-14-04`
