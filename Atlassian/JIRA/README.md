# Atlassian JIRA Installation

**Installing JIRA as a standalone**

Install sudo & git (if not already installed): `apt-get install -y sudo`

Install postgresql: `apt-get install -y postgresql postgresql-client`

Login to postgresql with: `sudo -u postgres psql template1`
* Set the password for user _postgres_ with `ALTER USER postgres with encrypted password 'yourpassword';` and exit psql using _Ctrl-D_
* Alter the `pghba.conf` with `nano /etc/postgresql/<postgresql_VERSION>/main/pg_hba.conf` on the line referring to _local all postgres_ from `peer` to `md5`.

Restart postgresql to apply settings: `service postgresql restart`

Create a new postgres user with `createuser -U postgres -d -e -E -l -P -r -s 'jirauser';` follow the prompt to setup your new user password.
* Alter the `pghba.conf` with `nano /etc/postgresql/<postgresql_VERSION>/main/pg_hba.conf` on the line referring to _local all all_ from `peer` to `md5`.

Restart postgresql to apply settings: `service postgresql restart`


Login to postgresql with your new user `psql -U jirauser template1`

* Create your bitbucket database ( in this example _bitbucketdb_ ) with `CREATE DATABASE jiradb WITH ENCODING 'UNICODE' LC_COLLATE 'C' LC_CTYPE 'C' TEMPLATE template0;`
* Grant all privileges to the newly created database with `GRANT ALL PRIVILEGES ON DATABASE jiradb TO 'jirauser';`


Download JIRA by: `wget https://www.atlassian.com/software/jira/downloads/binary/atlassian-jira-software-7.5.0-x64.bin` (Link needs to be updated with time, can get it from _https://www.atlassian.com/software/jira/download_)

Make the bin file executable: `chmod a+x atlassian-jira-software-x.x.x-x64.bin`

Launch the Jira installation as root:  `./atlassian-jira-software-x.x.x-x64.bin`

Once process is completed got to _http://IPOFSERVER:7990_
