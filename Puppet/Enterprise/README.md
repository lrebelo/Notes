# Puppet Enterprise Installation

----
## Installation
### Ubuntu 16.04

Make sure your Ubuntu server has a valid hostname ( name.domain ) by typing `hostname -f` the outcome should look like `myserver.mydomain`

Download Puppet Enterpise fro Ubuntu 16.04 from the official link using:
`curl -L -o pe.tgz 'https://pm.puppetlabs.com/cgi-bin/download.cgi?dist=ubuntu&rel=16.04&arch=amd64&ver=latest`

Extract the downloaded tarball with:
`tar -xvf pe.tgz and move into the extracted folder cd puppet-enterprise-2017.3.1-ubuntu-16.04-amd64.`

Run the installer as sudo ./puppet-enterprise-installer .

Once the installer is done, the shell will provide you with the url where you can complete the setup of your Puppet instance; that should look like:
`https://myseerver.mydomain:3000`

Open that url in your browser and start the setup. Provide the FQDN for the server and set a password for your admin account. On completion, you should have a running instance of Puppet enterprise .

To add nodes to it, copy the command provided under Setup/Unsigned certificates and run it in any node you want to sync with Puppet.

----

----
### Using a GIT repository

To manage environments with a control repository, you need to configure the Enterprise instance to work with a Git repository.

First off, create a control repository on your Git server and populate it with the Puppet template control repo ( this will give you a basic structure to work with ).
This repository comes with a Production environment already set.
If you need to add new environments, just create new branches.
Puppet provides a complete how-to guide on this link.
Make sure to have a user with the right access privileges to the repository.

On the puppet server, create an SSH key to allow control of the repository with:
`sudo ssh-keygen -t rsa -C "repository_user_emailaddress"`
make sure to create the key in a puppet folder and name it accordingly ( **/etc/puppetlabs/puppetserver/ssh/id-control_repo.rsa** ).
Also, make sure the folder and the key are available to the **pe-puppet** user ( check your file permissions ).

Copy the **/etc/puppetlabs/puppetserver/ssh/id-control_repo.rsa.pub** to your Git repository to allow read/write access to it.

In Puppet Enterprise, in classification, you want to find the PE Master ( *under PE Infrastructure* ). Under configuration, look for **puppet_enterprise::profile::master** and add the following:
code_manager_auto_configure set to true, r10k_remote set to your git repository ( **ssh://git@192.168.0.1/pup/control-repo.git** ), r10k_private_key to look at your ssh key ( **/etc/puppetlabs/puppetserver/ssh/id-control_repo.rsa** ).

At this stage you want to run the master node ( from the Run menu, puppet, Select node and picking your master node ).

In the Puppet Enterprise access control, create a new user that will be your deployment user. Make it part of the Code Deployers group and set it with a password.

Create a token to allow the newly created user to deploy code on the puppet server run `puppet-access login --service-url https://ip_or_hostname_of_your_puppet_server:4433/rbac-api --lifetime 180d`
The lifetime flag will make the token last for 180 days.

Test the deployment functionality running puppet-code deploy --dry-run . The outcome should look like the following:
```
--dry-run implies --wait.
--dry-run implies --all.
Dry-run deploying all environments.
Found 1 environments.
```
At this stage, run `puppet-code deploy production --wait` to test deployment of your production environment.
