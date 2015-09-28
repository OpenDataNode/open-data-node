Install and Upgrade Manual
---

[TOC]

## Configuration of system

Supported distribution is Debian Wheezy (7.x).

### Configure FQDN on Debian

* if [FQDN](http://en.wikipedia.org/wiki/Fully_qualified_domain_name) is defined, the host has to be visible from external network, (e.g. if FQDN is odn.example.com then the host must be visible both through http and https protocols via http://odn.example.com and https://odn.example.com)
* if [FQDN](http://en.wikipedia.org/wiki/Fully_qualified_domain_name) is not defined, the host has to be visible from external network, (e.g. if hostname is example then the host must be visible both through http and https via http://example and https://example)

#### Steps to configure FQDN

1. to get ip address of host <br>
```IP_ADDRESS=`/sbin/ifconfig eth0 | grep 'inet addr:' | cut -d: -f2 | awk '{ print $1}'` ```
2. to set FQDN - where host is accessible via  my-computer.my-domain.ext <br>
```echo "$IP_ADDRESS  my-computer.my-domain.ext my-computer " >> /etc/hosts```
3. to verify if host is set properly <br>
```hostname -A```

### Configuration of java for tomcat
tomcat 7 uses by default java 6 so it is necessary to change default java for tomcat. Edit /etc/default/tomcat7, update environment variable JAVA_HOME.
`JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64`

## Installation on clean system

To install packages from COMSODE Debian repository, please follow these steps:
 
1. Add ODN packages repository into apt-sources-list: <br>
`echo "deb http://packages.comsode.eu/debian/ wheezy main" > /etc/apt/sources.list.d/odn.list`

2. Add ODN public key: <br>
`wget -O - http://packages.comsode.eu/key/odn.gpg.key | apt-key add -`

3. Update apt sources: <br>
`aptitude update`

4. install ODN box (odn-simple package): <br>
`aptitude install odn-simple` <br>
The following user input is required during the installation process:
 * ldap password: whatever password can be used
 * Virtuoso password: dba

*__Note 1__: If you want to reduce packages that are not necessary, then call* <br>
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

## Installation of optional components

OpenDataNode supports also aditional components that could be installed and integrated with other core OpenDataNode components

### UnifiedViews QualityAssesment Plugins

To isntall QualityAssessment plugins, run following command
```
aptitude install qa-uv-plugins
```
It iwll install plugins into UnifiedViews.

### LDVMI (Payola)

Installation of LDVMI is not fuly automated, manual steps are necessary. InternalCatalog and  PublicCatalog have to be manualy configured to allow integration of visualization data in catalogs, ckan plugin `webpage_view` have to be enabled. See  further details about [webpage_view ckan plugin](http://docs.ckan.org/en/ckan-2.3.1/maintaining/data-viewer.html#web-page-view).
Detailed steps about configuration of ckan_plugins are in [CKAN documentation for ckan_plugins](http://docs.ckan.org/en/ckan-2.3.1/maintaining/configuration.html#ckan-plugins).

1. To install LDVMI (Payola) component, simply run:
```
aptitude install ldvmi
```
2. To allow visualization in InternalCatalog, edit file `/etc/odn-simple/odn-ckan-ic/production.ini`, add `webpage_view` into `ckan_plugins` parameter
```
ckan.plugins = ... webpage_view ...
```
3. In InternalCatalog configuration file, enable webpage_view to be default view [see details in ckan doc](http://docs.ckan.org/en/ckan-2.3.1/maintaining/configuration.html#ckan-views-default-views)
```
ckan.views.default_views = ... webpage_view ...
```
4. Repeat steps 2. and 3. for PublicCatalog in file `/etc/odn-simple/odn-ckan-pc/production.ini`
5. Restart InternalCatalog and PublicCatalog:
```
service apache2 restart
```
