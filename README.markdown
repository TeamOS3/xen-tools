xen-tools
=========

[![Travis CI Build Status](https://api.travis-ci.org/xen-tools/xen-tools.svg)](https://travis-ci.org/xen-tools/xen-tools)

* [Homepage](http://www.xen-tools.org/software/xen-tools)
* Git Repositories:
  * [at GitLab](http://gitlab.com/xen-tools/xen-tools)
  * [at GitHub](http://github.com/xen-tools/xen-tools)
  * [at Gitorious](http://gitorious.org/xen-tools) (deprecated)
* [Mailing Lists](http://www.xen-tools.org/software/xen-tools/lists.html)

About
-----

xen-tools contains a collection of Perl scripts for working with Xen
guest images under Linux.

Using this software, you can easily create new
[Xen](http://www.xen.org) guests configured to be accessible over the
network via [OpenSSH](http://www.openssh.org/).

xen-tools currently has scripts to install most releases of
[Debian](http://www.debian.org/) (starting with 3.1 "Sarge") and
[Ubuntu](http://www.ubuntu.com/) (starting with 6.06 LTS "Dapper") and
some RPM-based distributions. On the Dom0 side all current Xen
supporting distributions should work.

However, currently only Debian and Ubuntu releases are tested and
known to work reliably, i.e.:

### Debian

* Sarge 3.1 (i386 and DomU only)
* Etch 4.0 (Dom0 no more tested)
* Lenny 5.0
* Squeeze 6.0
* Wheezy 7
* Jessie 8
* Stretch 9 (under development)
* Sid (always under development; works at least at the moment of writing :-)

### Ubuntu

(only DomUs tested)

* Dapper Drake 6.06 (needs editing of `/usr/share/debootstrap/scripts/dapper` since at least debootstrap version 1.0.37, see [#659360][1])
* Edgy Eft 6.10 (needs editing of `/usr/share/debootstrap/scripts/edgy` since at least debootstrap version 1.0.37, see [#659360][1])
* Feisty Fawn 7.04
* Gutsy Gibbon 7.10
* Hardy Heron 8.04 (see [Installing Ubuntu 8.04 as DomU][2])
* Interpid Ibex 8.10
* Jaunty Jackaplope 9.04
* Karmic Koala 9.10
* Lucid Lynx 10.04
* Maverick Meerkat 10.10
* Natty Narwhal 11.04
* Oneiric Ocelot 11.10
* Precise Pangolin 12.04
* Quantal Quetzal 12.10
* Raring Ringtail 13.04
* Saucy Salamander 13.10
* Trusty Tahr 14.04 (LTS)
* Utopic Unicorn 14.10
* Vivid Vervet 15.04
* Wily Werewolf 15.10 (under development)

[1]: http://bugs.debian.org/659360
    "debootstrap in Wheezy can no more build Ubuntu Edgy or earlier"

[2]: http://www.linux-vserver.org/Installing_Ubuntu_8.04_Hardy_as_guest
    "There is an issue with debootstrap on hardy not installing ksyslogd."

### CentOS

(only DomUs tested, pygrub support incomplete)

* CentOS 5
* CentOS 6

Packages
--------

xen-tools are available prepackaged in Debian (and derivates) and as
source tar-ball for local installation. Installing from source should
work flawlessly on most Linux systems that meet the installation
requirements.

Requirements
------------

To use these tools you'll need the following software:

* [debootstrap](http://packages.debian.org/debootstrap)
* Perl and the following Perl modules
  * [Config::IniFiles](http://metacpan.org/release/Config-IniFiles)
	([Debian Package libconfig-inifiles-perl](http://packages.debian.org/libconfig-inifiles-perl))
  * [Text::Template](http://metacpan.org/release/Text-Template)
	([Debian Package libtext-template-perl](http://packages.debian.org/libtext-template-perl))
  * [Data::Validate::Domain](http://metacpan.org/release/Data-Validate-Domain)
	([Debian Package libdata-validate-domain-perl](http://packages.debian.org/libdata-validate-domain-perl))
  * [Data::Validate::IP](http://metacpan.org/release/Data-Validate-IP)
	([Debian Package libdata-validate-ip-perl](http://packages.debian.org/libdata-validate-ip-perl))
  * [Data::Validate::URI](http://metacpan.org/release/Data-Validate-URI)
	([Debian Package libdata-validate-uri-perl](http://packages.debian.org/libdata-validate-uri-perl))
  * [File::Slurp](http://metacpan.org/release/File-Slurp)
	([Debian Package libfile-slurp-perl](http://packages.debian.org/libfile-slurp-perl))
  * [File::Which](http://metacpan.org/release/File-Which)
	([Debian Package libfile-which-perl](http://packages.debian.org/libfile-which-perl))
  * and some more modules which are part of the Perl core and hence do not need to be installed separately.
* Make, if you are not installing through a package manager

You can try to install RPM-based distributions such as CentOS, or
Fedora Core, but you will need a correctly installed and configured
[rinse](http://packages.debian.org/rinse) package. This is currently
not fully supported.

If you wish to create new Xen instances which may be controlled by
users via a login shell you can have a look at the (currently
unmaintained) [xen-shell](http://xen-tools.org/software/xen-shell/)
project.


Installation
------------

As root or with sudo, execute `make install`.

See `debian/README.source` how to build the Debian package from a
checked out copy of the git repository (i.e. without a source tar
ball).


The Scripts
-----------

Here is a brief description of each included script, for more thorough
documentation please consult the appropriate man page.


### xen-create-image

This script is designed to create new images which may be used
with the Xen hypervisor.

This script performs the initial setup, then delegates the real
work to a collection of helper scripts:

* `xt-install-image`: Installs a distribution into a directory.

* `xt-customize-image`: Run a collection of hook scripts to configure
  the freshly installed system.

* `xt-create-xen-config`: Create a configuration file in `/etc/xen`
  such that Xen can boot the newly created machine.

* `xt-guess-suite-and-mirror`: In case of a Debian or Ubuntu Dom0,
  this script tries to guess the most suitable suite and mirror for
  DomUs based on the Dom0's `/etc/apt/sources.list`.


### xen-create-nfs

This script is similar in spirit to `xen-create-image`, but much less
complex.  It allows the creation of Xen guests which are diskless,
mounting their root filesystem over a remote NFS-share.

There are not many options to tweak, but still a useful addition 
to the suite.


### xen-delete-image

This script will allow you to completely remove Xen instances which
have previously been created by `xen-create-image`, this includes
removing the storage block devices from the system, and deleting the
Xen configuration file.


### xen-list-images

List all the created images beneath a given root directory along with
a brief overview of their setup details.


### xen-update-image

This script runs "apt-get update; apt-get upgrade" for a given Xen
image.

#### NOTES

* The image should not be running or corruption will occur!
* The script should only be used for Xen instances of Debian or a
  Debian-derived distribution.

Version Numbering Scheme
------------------------

Since release 4.4, the version numbering scheme of xen-tools tries to
comply with the [Semantic Versioning](http://semver.org/)
specification, with the only exception that trailing zeroes are
omitted.

Between the releases 3.9 and 4.4, the version numbering scheme
followed roughly the same ideas, but less strict.

Test Suite Coverage
-------------------

[![Coverage Status](https://coveralls.io/repos/xen-tools/xen-tools/badge.svg?branch=master)](https://coveralls.io/r/xen-tools/xen-tools?branch=master)

Despite parts of the test suite are quite old, it only tests a small
fraction of what xen-tools can do. Some of the scripts currently could
only be tested on an actual Xen Dom0. Hence the
[code coverage of xen-tools' test suite is quite bad](https://coveralls.io/r/xen-tools/xen-tools).

-- 
The Xen-Tools Developer Team
