# $Id: Makefile 13420 2011-02-20 21:04:03Z bdwalton $
# TODO (release-critical prefixed with !, non release-critical with *)
#
NAME = mcollective
VERSION = 2.11.1
GARTYPE = v2

DEBUG_MERGING=1
DEBUG_PACKAGING=1

DESCRIPTION = Puppetlabs Marionette Collection
define BLURB
  The Marionette Collective, also known as MCollective, is a framework
  for building server orchestration or parallel job-execution systems
endef

# https://github.com/puppetlabs/marionette-collective/archive/2.11.1.tar.gz

# DISTNAME       = $(VERSION)
# NAME           = marionette-collective
# NAME           = 
MASTER_SITES   = https://github.com/puppetlabs/marionette-collective/archive/
# UPSTREAMNAME   = $MASTER_SITES)$(VERSION).tar.gz
DISTFILES      = $(DISTNAME).tar.gz
DISTFILES     += CSWmcollective.preinstall
DISTFILES     += mcollective
DISTFILES     += cswusergroup

# CONFIGURE_ARGS = $(DIRPATHS)

INITSMF   = $(sysconfdir)/init\.d/mcollective
USERGROUP = $(sysconfdir)/pkg/mcollective/cswusergroup

PACKAGES += CSWmcollective
SPKG_DESC_CSWmcollective = Mcollective orchestration tool
RUNTME_DEP_PACKAGES_CSWmcollective += CSWruby24
# PKGFILES_CSWmcollective = $(smfconfdir)/init\.d/mcollectived

ARCHALL = 1
sysconfdir     = /etc$(prefix)
localstatedir  = /var$(prefix)
rubysitelibdir = $(shell ruby -rrbconfig -e "puts Config::CONFIG['sitelibdir']")
plconfdir      = /etc/puppetlabs/mcollective
plplugdir      = /opt/puppetlabs/mcollective/plugins

CONFIGURE_SCRIPTS =
BUILD_SCRIPTS     =
TEST_SCRIPTS      =
INSTALL_SCRIPTS   = mcollective

CHECKPKG_OVERRIDES_CSWmcollective += file-with-bad-content

include gar/category.mk

install-mcollective:

	# ugly workaround for "gem"
	test -d $(DESTDIR)/opt/puppetlabs/puppet/bin || mkdir -p $(DESTDIR)/opt/puppetlabs/puppet/bin
	test -f $(DESTDIR)/opt/puppetlabs/puppet/bin/gem || \
		ln -s /opt/csw/bin/gem $(DESTDIR)/opt/puppetlabs/puppet/bin/gem

	ginstall -m 755 -d $(DESTDIR)
	ginstall -m 755 -d $(DESTDIR)$(sysconfdir)
	ginstall -m 755 -d $(DESTDIR)$(sysconfdir)/init.d
	ginstall -m 755 -d $(DESTDIR)$(sysconfdir)/pkg/mcollective
	ginstall -m 755 $(FILEDIR)/mcollective $(DESTDIR)$(sysconfdir)/init.d
	ginstall -m 755 -d $(DESTDIR)$(localstatedir)/mcollective/run
	ginstall -m 755 -d $(DESTDIR)$(localstatedir)/lib
	ginstall -m 644 $(FILEDIR)/cswusergroup \
		$(DESTDIR)$(sysconfdir)/pkg/mcollective/cswusergroup
	ginstall -m 755 $(FILEDIR)/mcollective $(DESTDIR)$(sysconfdir)/init.d
	ginstall -m 755 -d $(DESTDIR)$(plplugdir)
	ginstall -m 755 -d $(DESTDIR)$(plconfdir)
	cd $(WORKSRC)/.. && mv marionette-collective-$(VERSION)/* mcollective-$(VERSION)
	cd $(WORKSRC) && ruby install.rb \
		--destdir=$(DESTDIR) \
		--configdir=$(plconfdir) \
		--bindir=$(bindir) \
		--sbindir=$(sbindir) \
		--full
	# find $(DESTDIR)$(bindir) -type f -depth -print | xargs -n 1 gsed -i -e '1 s|ruby$$|ruby21|g'
	@$(MAKECOOKIE)
