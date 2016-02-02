Install and Upgrade Manual
---

- [Configuration of system](#configuration-of-system)
	- [Configure FQDN on Debian](#configure-fqdn-on-debian)
	- [Configuration of java for tomcat](#configuration-of-java-for-tomcat)
- [Installation on clean system](#installation-on-clean-system)
	- [Post-installation steps](#post-installation-steps)
- [Upgrade from a previous version](#upgrade-from-a-previous-version)
- [Installation of optional components](#installation-of-optional-components)
	- [UnifiedViews QualityAssesment Plugins](#unifiedviews-qualityassesment-plugins)
	- [LDVMI (Payola)](#ldvmi-payola)
- [Support contacts](#support-contacts)

## Configuration of system

Supported distribution is Debian Wheezy (7.x).

### Configure FQDN on Debian

Before installing ODN, make sure that value returned by `hostname -f` is the one under which you would like your ODN instance to be accessible.
Example: If `hostname -f` returns "odn.myorganization.org", your ODN instance will be available at http://odn.myorganization.org/ .
Note: If the hostname is notproper FQDN, some users may experience problems while accessing your ODN instance.
If you are not sure how to configure FQDN on Debian system, please follow instructions at https://wiki.debian.org/HowTo/ChangeHostname .

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

### Post-installation steps

immediately after the installation is done perform the following steps:
* Open browser and use link `https://<hostname>/midpoint/login` where `<hostname>` is name of ODN server.
* Username: `administrator`
  Password: `5ecr3t`
* in the Users menu of ODN/MP create new user(s) (you can define an organization for every new user)

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

1. To install LDVMI (Payola) component, simply run:<br>
`aptitude install ldvmi`

2. To allow visualization in InternalCatalog, edit file `/etc/odn-simple/odn-ckan-ic/production.ini`, add `webpage_view` into `ckan_plugins` parameter<br>
`ckan.plugins = ... webpage_view ...`

3. In InternalCatalog configuration file, enable webpage_view to be default view [see details in ckan doc](http://docs.ckan.org/en/ckan-2.3.1/maintaining/configuration.html#ckan-views-default-views)<br>
`ckan.views.default_views = ... webpage_view ...`

4. Repeat steps 2. and 3. for PublicCatalog in file `/etc/odn-simple/odn-ckan-pc/production.ini`
5. Restart InternalCatalog and PublicCatalog:
```
service apache2 restart
```

## Support contacts

In case you encounter problems with installation or upgrade of ODN, please use mailing list odn-support@googlegroups.com . This mailing list is public and is preferred way of getting support.

If you need to post a message containng some more sensitive information, you can mail the ODN team using address support@opendatanode.org .
