# NextCloud

### Ubuntu SNAP install

#### Configuration with nginx reverse proxy

##### nginx config

Add 
```
proxy_set_header Host            $host;
proxy_set_header X-Real-IP       $proxy_protocol_addr;
proxy_set_header X-Forwarded-For $proxy_protocol_addr;
server_tokens off;
client_max_body_size 10000M;

proxy_set_header X-Content-Type-Options nosniff;
proxy_set_header X-XSS-Protection "1; mode=block";
proxy_set_header X-Robots-Tag none;
proxy_set_header X-Download-Options noopen;
proxy_set_header X-Permitted-Cross-Domain-Policies none;
proxy_set_header Referrer-Policy no-referrer;
```

##### Nextcloud config
When using an nginx reverse proxy with https.. 

In the `/var/snap/nextcloud/current/nextcloud/config/config.php` 
we need to add the line
```'overwriteprotocol' => 'https',``` 

Add: 
```'trusted_domains' =>
  array (
    0 => 'nexcloud.mydomain.meow',
  ),
  ```




### Debian 9.1 installation
Install pre-requisites:

`apt-get install -y apache2 mariadb-server libapache2-mod-php7.0`
`apt-get install -y php7.0-gd php7.0-json php7.0-mysql php7.0-curl php7.0-mbstring php7.0-intl php7.0-mcrypt php-imagick php7.0-xml php7.0-zip`


Download nextcloud: (we can get the link for the newer version from: `https://download.nextcloud.com/server/releases/`)

`wget https://download.nextcloud.com/server/releases/nextcloud-12.0.3.tar.bz2`
`tar -C /var/www -xvjf nextcloud-12.0.3.tar.bz2`


Some permissions may be missing run the following script.
`chown -R www-data:www-data /var/www/html/nextcloud`

Next configure apache2

`nano /etc/apache2/sites-available/nextcloud.conf`
add:
```
<VirtualHost *:80>
  ServerAdmin admin@example.com
  DocumentRoot "/var/www/html/nextcloud"
  ServerName 192.168.1.22

  <Directory "/var/www/html/nextcloud/">
    Options MultiViews FollowSymlinks

    AllowOverride All
    Order allow,deny
    Allow from all
  </Directory>
  TransferLog /var/log/apache2/nextcloud_access.log
  ErrorLog /var/log/apache2/nextcloud_error.log
</VirtualHost>
```

`a2dissite 000-default`
`a2ensite nextcloud`
`a2enmod rewrite`
`systemctl reload apache2`

Login to mariaDB and create database

`mysql -u root -p`
On the sql console:
`CREATE DATABASE nextcloud;`
`GRANT ALL ON nextcloud.* to 'nextcloud'@'localhost' IDENTIFIED BY 'NextCloudPassword';`
`FLUSH PRIVILEGES;`


Go to address and configure the nextcloud installation.


---
##### References

```
https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-nextcloud-on-ubuntu-16-04

https://www.howtoforge.com/tutorial/install-nextcloud-server-and-client-on-debian-9/
```
