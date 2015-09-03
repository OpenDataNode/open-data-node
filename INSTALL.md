Install and Upgrade Manual
---

Supported distribution is Debian Wheezy (7.x).

## Configuration of system

### Configure FQDN on Debian

* if [FQDN](http://en.wikipedia.org/wiki/Fully_qualified_domain_name) is defined, the host has to be visible from external network, (e.g. if FQDN is odn.example.com then the host must be visible both through http and https protocols via http://odn.example.com and https://odn.example.com)
* if [FQDN](http://en.wikipedia.org/wiki/Fully_qualified_domain_name) is not defined, the host has to be visible from external network, (e.g. if hostname is example then the host must be visible both through http and https via http://example and https://example)

Steps to configure FQDN
1. to get ip address of host
```
IP_ADDRESS=`/sbin/ifconfig eth0 | grep 'inet addr:' | cut -d: -f2 | awk '{ print $1}'`
```
1. to set FQDN - where host is accessible via  my-computer.my-domain.ext
```
echo "$IP_ADDRESS  my-computer.my-domain.ext my-computer " >> /etc/hosts
```
1. to verify if host is set properly
```
hostname -A
```

### Configuration of java for tomcat
tomcat 7 uses by default java 6 so it is necessary to change default java for tomcat. Edit /etc/default/tomcat7, update environment variable JAVA_HOME.
`JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64`

## Installation on clean system

To install packages from COMSODE Debian repository, please follow these steps:
 
1. Add ODN packages repository into apt-sources-list:
`echo "deb http://packages.comsode.eu/debian/ wheezy main" > /etc/apt/sources.list.d/odn.list`

2. Add ODN public key:
`wget -O - http://packages.comsode.eu/key/odn.gpg.key | apt-key add -`

3. Update apt sources:
`aptitude update`

4. install ODN box (odn-simple package):
`aptitude install odn-simple`
The following user input is required during the installation process:
 * ldap password: whatever password can be used
 * Virtuoso password: dba

*__Note 1__: If you want to reduce packages that are not necessary, then call*
```aptitude install --without-recommends odn-simple```

*__Note 2__: In case of fresh clean Debian Wheezy installation the steps described above should work, no other commands are required. However in some cases some python dependency problems has been detected (when the Debian environment was created as a result of virtualization).
In such cases the following steps are required to resolve the dependency problems:*
```
apt-get purge python\*
apt-get install python2.7-minimal  -V
apt-get install libapache2-mod-wsgi -V
apt-get install lsb-release  -V
apt-get install python-pkg-resources  -V
apt-get install python-pip  -V
apt-get install python-setuptools  -V
aptitude install odn-simple -V
```

## Upgrade from a previous version

*__Note__: :exclamation: Upgrade from ODN prior to 1.0.0 is not correctly supported due to change of CKAN version and also infrastructure changes.*

If you already installed ODN using steps described in previous section, follow these steps

1. it is strongly recommended to backup databases
2. to update to newest version, simply run:
```
aptitude update
aptitude upgrade
```
User is required to confirm replacement of configuration files from previous installation, it should be confirmed by pressing **'Y'** each time user input is required.
