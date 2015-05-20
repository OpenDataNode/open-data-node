# Open Data Node [![Stories in Ready](https://badge.waffle.io/opendatanode/open-data-node.svg?label=status:%20planed&title=Planed)](http://waffle.io/opendatanode/open-data-node) [![Stories in Ready](https://badge.waffle.io/opendatanode/open-data-node.svg?label=status:%20in%20progress&title=In%20Progress)](http://waffle.io/opendatanode/open-data-node) [![Stories in Ready](https://badge.waffle.io/opendatanode/open-data-node.svg?label=status:%20ready%20for%20test&title=Ready%20for%20test)](http://waffle.io/opendatanode/open-data-node) 

## Intro

For initial introduction of what Open Data Node is (or is supposed to be)
please read [a post on COMSODE
blog](http://www.comsode.eu/index.php/2014/06/open-data-node-what-it-is-what-it-does-what-is-next/).

## Documentation

As part of [COMSODE project](http://www.comsode.eu/), we're taking ideas from
[old Open Data Node](http://opendata.sk/liferay/open-data-node), existing
[UnifiedViews](https://github.com/UnifiedViews/) and we will put together new
Open Data Node.

Documentation will be located in following places (ordered based on
impotance and preference):

1. source code tree on GitHub: https://github.com/OpenDataNode/open-data-node
2. wiki on GitHub: https://github.com/OpenDataNode/open-data-node/wiki
3. Open Data Node in OpenData.sk Wiki: https://utopia.sk/wiki/display/ODN/

To make sure code matches documentation, having it together with source code
is good.  If that is not feasible for some reason, the next best thing is
wiki on GitHub.  Even if that is not feasible, you can use space in
OpenData.sk's Confluence Wiki.

### Architecture and Design

Planned architecture of Open Data Node is maintaned in Public Confluence
Wiki at:

https://utopia.sk/wiki/display/ODN/Architecture+and+Design


Currently it described mainly stuff mainly "as planned". LAter on, in line
with developoment, documentation will be maintaned to describe ODN "as is".

### Installation

Supported distribution is Debian Wheezy. To install packages from COMSODE Debian repository, please follow these steps:

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

## License

ODN will consists from several modules, each with its own license. But
overall, ODN will be [free and
open-source](https://en.wikipedia.org/wiki/FOSS).

For more information, please see:

https://utopia.sk/wiki/display/ODN/Architecture+and+Design+-+Licensing
