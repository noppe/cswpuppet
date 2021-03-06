# Copyright 2012 OpenCSW
# Distributed under the terms of the GNU General Public License v2
# $Id$

NAME = puppet4
VERSION = 4.10.4
GARTYPE = v2

define BLURB
  Puppet helps accomplish the goal of a hands-off, automated infrastructure.
  The benefits of automated infrastructure go beyond policy-enforced
  consistency and auditing.  The impact of hardware failure and other disaster
  scenarios can be mitigated, as services can be quickly restored by Puppet. In
  conjunction with virtualizaton, the ability to reliably create new systems
  running consistent services can be leveraged to create autoscaling
  applications as well as test systems identical to production environments.
endef

# http://downloads.puppetlabs.com/puppet/puppet-4.10.4.tar.gz

DISTNAME = puppet-${VERSION}
MASTER_SITES = https://downloads.puppetlabs.com/puppet/
DISTFILES  = ${DISTNAME}.tar.gz
DISTFILES += cswpuppetd
# DISTFILES += cswpuppetmasterd
DISTFILES += cswusergroup
DISTFILES += CSWpuppet.postinstall
DISTFILES += README.CSW
DISTFILES += puppet.conf.example-CSW

SPKG_SOURCEURL = http://puppetlabs.com/
LICENSE_CSWpuppet4 = LICENSE
LICENSE_CSWpuppetmaster4 = LICENSE

INITSMF += $(sysconfdir)/init\.d/cswpuppetd
INITSMF += $(sysconfdir)/init\.d/cswpuppetmasterd
USERGROUP = $(sysconfdir)/pkg/puppet/cswusergroup

PACKAGES += CSWpuppet4
SPKG_DESC_CSWpuppet4 = System configuration management tool, client daemon
RUNTIME_DEP_PKGS_CSWpuppet4 += CSWfacter2
RUNTIME_DEP_PKGS_CSWpuppet4 += CSWhiera
RUNTIME_DEP_PKGS_CSWpuppet4 += CSWrubyaugeas
RUNTIME_DEP_PKGS_CSWpuppet4 += CSWruby24
BUILD_DEP_PKGS_CSWpuppet4 = $(RUNTIME_DEP_PKGS_CSWpuppet4)

# Creating a separate package CSWpuppetmaster so that the cswpuppetmaster
# service isn't started upon the installation of CSWpuppet.
PACKAGES += CSWpuppetmaster4
SPKG_DESC_CSWpuppetmaster4 = System configuration management tool, server
RUNTIME_DEP_PKGS_CSWpuppetmaster4 += CSWhiera
RUNTIME_DEP_PKGS_CSWpuppetmaster4 += CSWruby24
BUILD_DEP_PKGS_CSWpuppetmaster4 = $(RUNTIME_DEP_PKGS_CSWpuppetmaster4)

INCOMPATIBLE_PKGS += CSWpuppet
INCOMPATIBLE_PKGS += CSWpuppetmaster
INCOMPATIBLE_PKGS += CSWpuppet3
INCOMPATIBLE_PKGS += CSWpuppetmaster3
	     
PKGFILES_CSWpuppetmaster4  = $(sysconfdir)/init\.d/cswpuppetmasterd
PKGFILES_CSWpuppetmaster4 += $(sbindir)/puppetmasterd
PKGFILES_CSWpuppetmaster4 += $(mandir)/man8/puppetmasterd.8
PKGFILES_CSWpuppetmaster4 += $(libdir)/.*/puppet/application/puppetmasterd.rb
PKGFILES_CSWpuppetmaster4 += $(sysconfdir)/puppet/auth.conf

ARCHALL = 1

sysconfdir     = /etc$(prefix)
localstatedir  = /var$(prefix)
rubysitelibdir = $(shell ruby -rrbconfig -e "puts Config::CONFIG['sitelibdir']")

CONFIGURE_SCRIPTS =
BUILD_SCRIPTS	  =
TEST_SCRIPTS	  =
INSTALL_SCRIPTS	  = puppet

CHECKPKG_OVERRIDES_CSWpuppet4 += surplus-dependency|CSWfacter2
CHECKPKG_OVERRIDES_CSWpuppet4 += surplus-dependency|CSWhiera
CHECKPKG_OVERRIDES_CSWpuppet4 += surplus-dependency|CSWrubyaugeas
# Puppet has hardcoded paths to look at for many OSes, many of which involve
# /usr/share and /usr/local.
CHECKPKG_OVERRIDES_CSWpuppet4 += file-with-bad-content
# These files are quite deliberate - without them users will not get CSW
# behaviour. The README explains
CHECKPKG_OVERRIDES_CSWpuppet4 += bad-location-of-file|file=/etc/puppet
CHECKPKG_OVERRIDES_CSWpuppet4 += bad-location-of-file|file=/etc/puppet/README.CSW
CHECKPKG_OVERRIDES_CSWpuppet4 += bad-location-of-file|file=/etc/puppet/puppet.conf.example-CSW
CHECKPKG_OVERRIDES_CSWpuppet4 += bad-location-of-file|file=/var/lib
CHECKPKG_OVERRIDES_CSWpuppet4 += bad-location-of-file|file=/var/lib/puppet
# yes, yes, we know it conflicts with puppet{,master} - that's the point of
# INCOMPATIBLE_PKGS
CHECKPKG_OVERRIDES_CSWpuppet4 += file-collision

CHECKPKG_OVERRIDES_CSWpuppetmaster4 += surplus-dependency|CSWhiera
CHECKPKG_OVERRIDES_CSWpuppetmaster4 += surplus-dependency|CSWruby24
# yes, yes, we know it conflicts with puppet{,master} - that's the point of
# INCOMPATIBLE_PKGS
CHECKPKG_OVERRIDES_CSWpuppetmaster4 += file-collision

include gar/category.mk

install-puppet:
	ginstall -m 755 -d $(DESTDIR)
	ginstall -m 755 -d $(DESTDIR)/etc/puppet
	ginstall -m 755 -d $(DESTDIR)/var/lib/puppet
	ginstall -m 755 -d $(DESTDIR)$(sysconfdir)/puppet
	ginstall -m 755 -d $(DESTDIR)$(sysconfdir)/init.d
	ginstall -m 755 -d $(DESTDIR)$(sysconfdir)/pkg/puppet
	ginstall -m 755 -d $(DESTDIR)$(localstatedir)/puppet/run
	ginstall -m 755 -d $(DESTDIR)$(mandir)/man5
	ginstall -m 755 -d $(DESTDIR)$(mandir)/man8
	ginstall -m 755 $(FILEDIR)/cswpuppetd $(DESTDIR)$(sysconfdir)/init.d
	ginstall -m 644 $(WORKSRC)/conf/puppet.conf $(DESTDIR)/etc/puppet/puppet.conf.example

	cd $(WORKSRC) && ruby install.rb \
		--destdir=$(DESTDIR) \
		--configdir=$(sysconfdir)/puppet \
		--bindir=$(bindir) \
		--sbindir=$(sbindir) \
		--full
	# we depend on ruby2.1 so fix the shebang
	# find $(DESTDIR)$(bindir) -type f -depth -print | xargs -n 1 gsed -i -e '1 s|ruby$$|ruby24|g'
	cd $(DESTDIR)$(mandir)/man8 
	mv $(DESTDIR)$(sysconfdir)/puppet/auth.conf $(DESTDIR)$(sysconfdir)/puppet/auth.conf.example
	ginstall -m 644 $(FILEDIR)/puppet.conf.example-CSW $(DESTDIR)$(sysconfdir)/puppet/puppet.conf.example
	ginstall -d -m 755 $(DESTDIR)$(sysconfdir)/pkg/puppet
	ginstall -m 644 $(FILEDIR)/cswusergroup \
		$(DESTDIR)$(sysconfdir)/pkg/puppet/cswusergroup
	mkdir -p $(DESTDIR)$(localstatedir)/lib
	# gunzip $(DESTDIR)$(mandir)/man5/puppet.conf.5.gz
	@$(MAKECOOKIE)


