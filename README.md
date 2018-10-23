# baremetal-razor
Trying to build a controller on a Bare metal setting

## Useful links:
Overview 
https://puppet.com/docs/pe/2019.0/pe_architecture_overview.html

Bare Metal Provisioning with Razor in Puppet Enterprise 3.8

https://www.youtube.com/watch?v=7DgSSgjcaoc

https://www.youtube.com/watch?v=x43WsZLGGsc

## install 


[Install Puppet Enterprise -- Start guide](https://puppet.com/docs/pe/2019.0/install_pe_getting_started.html#install-puppet-enterprise-quick-start-guide)


## Glossary
Repo - this is the way to create "rules" to install a new OS from an ISO file.
E.g., 
razor create-repo --name trusty --iso-url { THE URL } 

Broker - ? 

### Finally: 
Policy: 
razor create-policy --name trusty-default --repo trust --hostname '${id}.example.org' --root-password secrete --broker puppet-pe 


# Setup
This toturial is a test enviroment setup with two VMs one will run the puppet server the other one will be used to install an OS using razor. 

1. Download [VirtualBox](https://www.virtualbox.org/wiki/Linux_Downloads)

2. Download your favorite Linux 

3. Configure the OS in the VM

4. Install PostgreSQL:
	* Download and install: ```$ sudo apt-get install postgresql```
	* Run PostgresSQL: ```$ sudo -u postgres psql```
	* create a user: ```postgres=# create user razor;```
	* create a database: ```postgres=#  CREATE DATABASE razor_prd OWNER razor;```
	* Edit file ```$ sudo nano /etc/postgresql/10/main/pg_hba.conf``` to remove password from Razor: local all razor_prd trust 

5.  Download Razor: ```wget https://apt.puppetlabs.com/puppet6-release-bionic.deb```
6.  Install Razor: ```dpkg -i puppet6-release-bionic.deb```



## Helpful links
[PostgrestSQL user commands](https://www.digitalocean.com/community/tutorials/how-to-use-roles-and-manage-grant-permissions-in-postgresql-on-a-vps--2)

