# $Id$
# TODO (release-critical prefixed with !, non release-critical with *)
#
NAME = hiera
VERSION = 1.3.4
GARTYPE = v2

DESCRIPTION = Brief description
define BLURB
  Long description
endef

PATCHFILES += 0001-change-config-dir-to-etc-opt-csw.patch

RUNTIME_DEP_PKGS = CSWruby24

MASTER_SITES = http://downloads.puppetlabs.com/hiera/
DISTFILES  = $(DISTNAME).tar.gz

CONFIGURE_ARGS = $(DIRPATHS)

ARCHALL = 1

CONFIGURE_SCRIPTS =
BUILD_SCRIPTS     =
TEST_SCRIPTS      =
INSTALL_SCRIPTS   = hiera

# currently checkpkg does not know anything about ruby24
CHECKPKG_OVERRIDES_CSWhiera += missing-dependency|CSWlibruby1-9-1-1|or|CSWruby|or|CSWruby18|or|CSWruby191

# we really depend on ruby24
CHECKPKG_OVERRIDES_CSWhiera += surplus-dependency|CSWruby24

include gar/category.mk

install-hiera:
	ginstall -m 755 -d $(DESTDIR)
	cd $(WORKSRC) && ruby install.rb \
		--destdir=$(DESTDIR) \
		--configdir=$(sysconfdir) \
		--bindir=$(bindir) \
		--full
	# we depend on ruby so fix the shebang
	# find $(DESTDIR)$(bindir) -type f -depth -print | xargs -n 1 gsed -i -e '1 s|ruby$$|ruby21|g'

