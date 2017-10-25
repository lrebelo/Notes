# Icinga2

## Installation
### Debian 9


#### Add rep

`wget -O - https://packages.icinga.com/icinga.key | apt-key add -`
`echo 'deb https://packages.icinga.com/debian icinga-stretch main' >/etc/apt/sources.list.d/icinga.list`
`apt-get update`

#### Install
* ... with: `apt-get install icinga2 -y`
* Add standard plug-ins: `apt-get install monitoring-plugins`

#### Run
* Start by:
  * `/etc/init.d/icinga2 start`
    *  or
  * `service icinga2 start`

* Enable service by:
  * `systemctl enable icinga2`

#### Icinga Web 2 Installation
* Install mysql & icinga2 tools:
  * `apt-get install mysql-server mysql-client`
  * `apt-get install icinga2-ido-mysql`
    * It will prompt for a new password at this point, this will be for the fetching of monitoring data.
* Install apache2
  * `apt-get install apache2`
* Set iptables
  * iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
* Enable API
  * `icinga2 api setup`
* Restart Icinga2
  * `systemctl restart icinga2`

* Install icingaweb2
  * `apt-get install icingaweb2`
* Create db table
  * `CREATE DATABASE icingaweb2;`
  * `GRANT ALL ON icingaweb2.* TO icingaweb2@localhost IDENTIFIED BY 'PASSWORD';`
* Create a token..
  * `icingacli setup token create`
    * Copy token for next stage..
* Browse to configuration page
  * `http://localhost/icingaweb2/setup`
    * Replace localhost with your Icinga machine ip.
  * Follow instruction..
  * First db config is for the icingaweb2 identified by user `icingaweb2` password `PASSWORD`
  * Second is for IDO, user `icinga2` password **your password**

* Done
