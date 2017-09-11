# Atlassian Bitbucket Installation

**Installing Bitbucket as a standalone**

Install sudo & git (if not already installed): `apt-get install -y sudo git`

Install postgresql: `apt-get install -y postgresql postgresql-client`

Login to postgresql with: `sudo -u postgres psql template1`
* Set the password for user _postgres_ with `ALTER USER postgres with encrypted password 'yourpassword';` and exit psql using _Ctrl-D_
* Alter the `pghba.conf` with `nano /etc/postgresql/<postgresql_VERSION>/main/pg_hba.conf` on the line referring to _local all postgres_ from `peer` to `md5`.

Restart postgresql to apply settings: `service postgresql restart`

Create a new postgres user with `createuser -U postgres -d -e -E -l -P -r -s 'bitbuckuser';` follow the prompt to setup your new user password.
* Alter the `pghba.conf` with `nano /etc/postgresql/<postgresql_VERSION>/main/pg_hba.conf` on the line referring to _local all all_ from `peer` to `md5`.

Restart postgresql to apply settings: `service postgresql restart`


Login to postgresql with your new user `psql -U bitbuckuser template1`

* Create your bitbucket database ( in this example _bitbucketdb_ ) with `CREATE DATABASE bitbucketdb WITH ENCODING 'UNICODE' LC_COLLATE 'C' LC_CTYPE 'C' TEMPLATE template0;`
* Grant all privileges to the newly created database with `GRANT ALL PRIVILEGES ON DATABASE bitbucketdb TO 'bitbuckuser';`


Download Bitbucket by: `wget https://www.atlassian.com/software/stash/downloads/binary/atlassian-bitbucket-5.3.1-x64.bin` (Link needs to be updated with time, can get it from _https://www.atlassian.com/software/bitbucket/download_)

Make the bin file executable: `chmod a+x atlassian-bitbucket-x.x.x-x64.bin`

Launch the Jira installation as root:  `./atlassian-bitbucket-x.x.x-x64.bin`

Once process is completed got to _http://IPOFSERVER:7990_

## Bitbucket under nginx proxy_pass
**using Bitbucket with nginx and ssl**

Add the folling lines to the file `/var/atlassian/application-data/bitbucket/shared/bitbucket.properties`
 If file does not exist, create it!

`server.port=7990
server.secure=true
server.scheme=https
server.proxy-port=443
server.proxy-name=<DOMAIN_OF_LINK>`

on the nginx configuration add:
`server {

    listen 443 ssl;

    server_name <FULLY_QUALIFIED_DOMAIN_NAME>;
    ssl_certificate /etc/nginx/ssl/nginx.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx.key;

    location /{
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

        proxy_pass <LOCATION_OF_BITBUCKET>;

        client_max_body_size 10M;
   }
}`
