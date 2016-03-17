Install and Upgrade Manual
---

- [Configuration of system](#configuration-of-system)
	- [Configure FQDN on Debian](#configure-fqdn-on-debian)
	- [Configuration of java for tomcat](#configuration-of-java-for-tomcat)
- [Installation on clean system](#installation-on-clean-system)
	- [Post-installation steps](#post-installation-steps)
- [Upgrade from a previous version](#upgrade-from-a-previous-version)
- [Upgrade from debian 7 to 8](#upgrade-from-debian-7-to-8)
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
tomcat 7 uses by default java 6 so it is necessary to change default java for tomcat. Edit `/etc/default/tomcat7`, update environment variable JAVA_HOME.
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

Immediately after the installation is done, please perform the following steps:

#### Reset default passwords in internal part

Log in into private part of ODN and get into user management module:

* Open browser and use link `https://<hostname>/midpoint/login` (where `<hostname>` is name/FQDN of your ODN server)
* Log in with user name `administrator` and password `5ecr3t`
* Select `Tools > User Management` to get into ODN/midPoint

Set new password for `admnistrator`:

* Select `List > Users`
* Select `administrator`
* Select `User details settings > Show empty fields` (`User details settings` is the "gear whell" icon right to the "User details")
* At the bottom, fill in new password (twice) and submit (`Save`)

And set new password also for `casadmin` by repeating last 4 steps, this time for user "casadmin".

#### Create accounts for usual work

While you are already present in user management part logged in as administrator, it is a good time to create regular non-administrative accounts for users which are going to regularly use ODN to publish data. Simply click `Users > New User`, fill in necessary details and submit. Repeat for each new user.

For more information, please take a look into ODN Administration Manual: https://utopia.sk/wiki/pages/viewpage.action?pageId=57672392

#### Reset default password in public part

Log in into public part of ODN:

* Open browser and use link `https://<hostname>/user/login` (where `<hostname>` is name/FQDN of your ODN server)
* Log in with user name `admin` and password `admin`
* Select `Edit settings` ("gear wheel" icon left to "logout" icon in top-right cornenr)
* At the bottom, fill in new password (twice) and submit (`Update Profile`)

## Upgrade from a previous version

*__Note__: :exclamation: Upgrade from ODN prior to 1.0.0 is not correctly supported due to change of CKAN version and also infrastructure changes.*

If you already installed ODN using steps described in previous section, follow these steps

1. it is strongly recommended to backup databases
2. to update to newest version, simply run:
```
apt-get update
apt-get upgrade
```
User is required to confirm replacement of configuration files from previous installation, it should be confirmed by pressing **'Y'** each time user input is required.

## Upgrade from debian 7 to 8 
When you finish a [upgrading](https://www.debian.org/releases/stable/i386/release-notes/ch-upgrading.en.html) process you shall upgrade odn as well. Use a repository for debian 8.

```
echo "deb http://packages.comsode.eu/debian/ jessie main" > /etc/apt/sources.list.d/odn.list
apt-get update
apt-get upgrade
```


*__Note__: :exclamation:  If you dealing with the problems mentioned down you can fix it by

`apt-get install --reinstall odn-simple`

* Internal catalog or public catalog is unavailable.  `https://<hostname>/` or `https://<hostname>/internalcatalog`
and you see an message ```ImportError: No module named _sysconfigdata_nd in /var/log/apache2/odn-ckan-ic.error```  
it's a known issue in Debian 8 - read more here https://bugs.launchpad.net/ubuntu/+source/python2.7/+bug/1115466


* If all services are unavailable `https://<hostname>/midpoint/` `https://<hostname>/` or `https://<hostname>/internalcatalog` `https://<hostname>/unifiedviews`
It's because apache2. it's a known issue in Debian 8 - read more here  https://www.debian.org/releases/stable/amd64/release-notes/ch-information.en.html#apache-httpd-incomat

* Internal catalog or public catalog is unavailable. `https://<hostname>/` or `https://<hostname>/internalcatalog`
and you see an message `**AttributeError: 'module' object has no attribute 'PROTOCOL_SSLv3'** `in `/var/log/apache2/odn-ckan-ic.error ` 
it's known issue in Debian 8 - read more here - http://stackoverflow.com/questions/28987891/patch-pyopenssl-for-sslv3-issue 


## Installation of optional components

Open Data Node supports also aditional components that could be installed and integrated with other core Open Data Node components

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
