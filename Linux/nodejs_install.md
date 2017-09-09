# Nodejs LTS (8.x) install for debian Linux 8


**I normally do this throught as root rather than sudo for spead sake when configuring on a server not a dev machine**

`curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -`

`sudo apt-get install nodejs_`

`apt-get install build-essential`

**IF using ElementaryOS Loki**

- go: `sudo nano /etc/apt/sources.list.d/nodesource.list` and change the distro name to xenial as the rep has no hook for loki


**If upgrading..**

`apt-get purge nodejs npm`
