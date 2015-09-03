Install and Upgrade Manual
---

Supported distribution is Debian Wheezy.

## Installation on debian

### Installation on clean system

To install packages from COMSODE Debian repository, please follow these steps:
 
1. Add ODN packages repository into apt-sources-list:
```
echo "deb http://packages.comsode.eu/debian/ wheezy main" > /etc/apt/sources.list.d/odn.list
```

2. Add ODN public key:
```
wget -O - http://packages.comsode.eu/key/odn.gpg.key | apt-key add -
```

3. Update apt sources:
```
aptitude update
```

4. install ODN box:
```
aptitude install odn-simple
```

### Upgrade from previous version

If you already installed ODN using steps described in previous section, simply run:
```
aptitude update
aptitude upgrade
```
