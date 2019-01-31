# baremetal-provisioning

I am trying to build a heteregenous replicated system using baremetal provision. 
Below are some solutions that should help:
* [Coobler](http://cobbler.github.io/manuals/quickstart/)
* [RackHD](https://github.com/rackhd/rackhd) [another](https://rackhd.readthedocs.io/en/latest/rackhd_overview.html)
* [xCat](http://xcat.org/)
* [*More*](https://devops.com/flap-part-1-server-provisioning/) and [*some more*](https://www.cyberciti.biz/tips/server-provisioning-software.html)

So far, Razor is the one with best results. 
It seems simples and it integrates with Puppet.
Before continuing, see this [video](https://www.youtube.com/watch?v=cR1bOg0IU5U) at a Puppet conference. 
It is the best video to explain what it is and how it works. 

# Razor
I have followed the Razor's github [instructions](https://github.com/puppetlabs/razor-server)
*always run ```$service razor-server start after restart```*
## Prepare the test envirorment
1. Donwload CentOS [image](http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-DVD-1804.iso) 
2. Create VM (at least: 50GB disk size, 1GB RAM, and 2 CPUs).
	* Update the machine ```$ sudo yum -y update```
	* Install GCC ```sudo yum install gcc```
	* Install alternative kernel to add Guest Additions: ```yum -y install kernel-devel.x86_64_0:3.10.0-862.el7``` (see which is suggested in the Guest Additions installer)
	* [Change](https://www.thegeekdiary.com/centos-rhel-7-change-default-kernel-boot-with-old-kernel/) the default kernel on the grub menu ```grub2-set-default 0``` and restart

## Installing Razor
Follow the instuctions [here](https://github.com/puppetlabs/razor-server/wiki/Installation) with the additonal commands below:
1. **Database Setup:** Install PostgresSQL ```sudo yum install postgresql-server postgresql-contrib```
	* Run [these](https://www.linode.com/docs/databases/postgresql/how-to-install-postgresql-relational-databases-on-centos-7/) instructions to create users and DBs
	* Change the permissions on Postgres [config file](https://unix.stackexchange.com/a/234334) like [this](https://stackoverflow.com/a/18664239/5077205) (change md5 to trust)
2. **Razor Server Setup:** Follow the comands 
3. **Donwload Microkernel:** ```wget -O microkernel-008.tar "http://pup.pt/razor-microkernel-latest"``` and extract  ```tar -xf microkernel-008.tar```
	* ```sudo nano /etc/puppetlabs/razor-server/config.yam``` edit repo path
4. **Razor Client Setup:** add razor to ```/etc/hosts/```
5. **PXE Setup:**
	* Change PXE if needed, [VMWare configuration](http://ipxe.org/howto/vmware) -- dowload Zdlib to build roms.
	* ```sudo yum -y install tftp-server.x86_64```
	* Configure DHCP ([follow the instructions](https://www.tecmint.com/install-dhcp-server-i21392139n-centos-rhel-fedora/)) DHCP [commands](https://www.cyberciti.biz/faq/starting-stopping-restarting-dhcpd-in-fedora-linux/). **Additional relevant** [info](https://technodrone.blogspot.com/2013/11/razor-dhcp-and-tftp.html):
		* The url is: *http://razor:8150/api/microkernel/bootstrap?nic_max=4*
		* ```sudo nano /etc/dhcp/dhcpd.conf```
		* ```sudo systemctl restart dhcpd.service```
		* ```journalctl -xe``` or  ```sudo systemctl status -l dhcpd```
	* Install VirtualBox extensions on the HOST ([donwload here](https://www.virtualbox.org/wiki/Downloads])) - [this](https://linuxacademy.com/community/posts/show/topic/7812-pxe-server-problem) problem take me a while to figure it out.
	* Needed to add the MAC to arp ? ``` sudo arp -s <IP> <MAC> -i <INTERFACE>```
	* Then remove some rules from iptables (they are not persisted):  ```sudo iptables -D INPUT -j REJECT --reject-with icmp-host-prohibited``` (this tip was taken from [here](https://openstack.nimeyo.com/88925/openstack-neutron-icmp-host-unreachable-admin-prohibited))

* ```sudo systemctl restart dnsmasq.service```
* ```sudo systemctl restart dhcpd.service```
* ```sudo service razor-server start```
* ```sudo setenforce 0``` Disable SE Linux temporarly [more info](https://linuxize.com/post/how-to-disable-selinux-on-centos-7/)
* Not sure about permissions and restarts. Sometimes a different combination may lead to a different result. 

## [Probably the best site with step by step](https://sites.google.com/site/mrxpalmeiras/puppet/razor-provisioning)


### Aditional commands:
Turn off firewall ```sudo systemctl disable firewalld```

### Current state:
Loaded the pxe bootstrap scritp, connected to razor but it says the file is too large.



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


