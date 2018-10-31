# baremetal-provisioning

I am trying to build a heteregenous replicated system using baremetal provision. 
Below are some solutions that should help:
* [Coobler](http://cobbler.github.io/manuals/quickstart/)
* [RackHD](https://github.com/rackhd/rackhd) [another](https://rackhd.readthedocs.io/en/latest/rackhd_overview.html)
* [xCat](http://xcat.org/)
* [*More*](https://devops.com/flap-part-1-server-provisioning/) and [*some more*](https://www.cyberciti.biz/tips/server-provisioning-software.html)

So far, Razor is the one with best results. 
It seems simples and it integrates with Puppet.
Before continuing see this [video](https://www.youtube.com/watch?v=cR1bOg0IU5U) at a Puppet conference. 
It is the best video to explain what it is and how it works. 

# Razor (via [github](https://github.com/puppetlabs/razor-server))

## Prepare the test envirorment
1. Donwload CentOS [image](http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-1804.iso) 
2. Create VM (at least: 50GB disk size, 1GB RAM, and 2 CPUs).
	* Update the machine ```$ sudo yum -y update```
	* Install GCC ```sudo yum install gcc```
	* Install alternative kernel to add Guest Additions: ```yum -y install kernel-devel.x86_64_0:3.10.0-862.el7``` (see which is suggested in the Guest Additions installer)
	* [Change](https://www.thegeekdiary.com/centos-rhel-7-change-default-kernel-boot-with-old-kernel/) the default kernel on the grub menu ```grub2-set-default 0``` and restart

## Installing Razor [here](https://github.com/puppetlabs/razor-server/wiki/Installation)
1. **Database Setup:** Install PostgresSQL ```sudo yum install postgresql-server postgresql-contrib```
	* Run [these](https://www.linode.com/docs/databases/postgresql/how-to-install-postgresql-relational-databases-on-centos-7/) instructions to create users and DBs
	* Change the permissions on Postgres [config file](https://unix.stackexchange.com/a/234334) like [this](https://stackoverflow.com/a/18664239/5077205) (change md5 to turst)
2. **Razor Server Setup:** Follow the comands 



## (DEPRECATED) CentoOS (2)
1. Donwload CentOS [image](http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-1804.iso) 
2. Create VM (at least: 50GB disk size, 1GB RAM, and 2 CPUs).
	* Update the machine ```$ sudo yum -y update```
	* Install GCC ```sudo yum install gcc```
	* Install alternative kernel to add Guest Additions: ```yum -y install kernel-devel.x86_64_0:3.10.0-862.el7``` (see which is suggested in the Guest Additions installer)
	* [Change](https://www.thegeekdiary.com/centos-rhel-7-change-default-kernel-boot-with-old-kernel/) the default kernel on the grub menu ```grub2-set-default 1``` and restart
3. Download [Puppet Enterprise](https://puppet.com/download-puppet-enterprise/thank-you) (need registration)
	* ```wget -O puppet-enterprise-2019.0.0-el-7-x86_64.tar.gz "https://pm.puppet.com/cgi-bin/download.cgi?arch=x86_64&dist=el&rel=7&ver=latest"```
4. Installing [Puppet Enterprise](https://puppet.com/docs/pe/2019.0/install_pe_getting_started.html#install-puppet-enterprise-quick-start-guide)
5. After the installation go to the https://localhost.localdomain:3000 and continue the installation.
6. Add a DNS entry for puppet ```sudo nano /etc/hotsts/``` in the end of the line of 127.0.0.1
7. At the end of the installation go to https://localhost.localdomain/auth/login?redirect=/ (user default login: *admin*)
	* Follow the instructions [here](https://puppet.com/docs/pe/2019.0/install_nix_agents_getting_started_guide.html#install-nix-agents-quick-start-guide)
	* Then, run  ```sudo /opt/puppetlabs/bin/puppet agent -t```
8. Following this https://puppet.com/docs/pe/2019.0/installing_razor.html -- however, I am not sure that this works... 



## Ubuntu
#### Install
1. Donwload Puppet Enterprise for your distribution [here](https://puppet.com/download-puppet-enterprise):

```$ wget -O puppet-enterprise-2019.0.0-ubuntu-18.04-amd64.tar.gz "https://pm.puppet.com/cgi-bin/download.cgi?arch=amd64&dist=ubuntu&rel=18.04&ver=latest"```


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

# Foreman

This toturial is a test enviroment setup with two VMs one will run the puppet server the other one will be used to install an OS using razor. 

1. Download [VirtualBox](https://www.virtualbox.org/wiki/Linux_Downloads)
2. Download your favorite Linux 
3. Configure the OS in the VM it needs 4GB ram
4. ... [instalation guide](https://www.theforeman.org/introduction.html)
5. Change /etc/hosts and replace 127.0.0.1 for the real IP: 1.2.3.4 foreman.lasige.di.fc.ul.pt (full domain if needed). 
In the end the output of ```facter fqdn``` needs to be the same as ```ping $(hostname -f)```
6. Output from installer not used yet: admin / qKSHepQom85rTgFg 


### supported OSes (guests)
* Red Hat Enterprise Linux 
* CentOS
* Fedora
* Ubuntu
* Debian
* Solaris 8, 10
* OpenSUSE  
* SLES 
* Oracle Linux
* CoreOS
* FreeBSD
* Junos

## install 


