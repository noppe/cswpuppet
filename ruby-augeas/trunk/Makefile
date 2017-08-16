NAME       = ruby-augeas
VERSION    = 0.5.0
GARTYPE = v2

DESCRIPTION = Augeas Ruby Bindings
define BLURB
endef

MASTER_SITES = http://download.augeas.net/ruby/
DISTFILES    = $(NAME)-$(VERSION).tgz
VENDOR_URL   = http://augeas.net

PACKAGES    = CSWrubyaugeas
CATALOGNAME = ruby_augeas

RUNTIME_DEP_PKGS_CSWrubyaugeas += CSWruby24
RUNTIME_DEP_PKGS_CSWrubyaugeas += CSWlibaugeas0
RUNTIME_DEP_PKGS_CSWrubyaugeas += CSWlibxml2-2

CHECKPKG_OVERRIDES_CSWrubyaugeas += catalogname-does-not-match-pkgname|pkgname=CSWrubyaugeas|catalogname=ruby_augeas|expected-catalogname=rubyaugeas

PATCHFILES += 0001-use-gnu-make-for-building-_augeas.so.patch
PATCHFILES += 0002-modify-LDFLAGS-to-add-opt-csw-lib-to-the-runtime-lin.patch

BUILD_DEP_PKGS += $(RUNTIME_DEP_PKGS_CSWrubyaugeas)
# BUILD_DEP_PKGS += CSWruby24-dev
BUILD_DEP_PKGS += CSWlibaugeas-dev

BUILD_SCRIPTS     =
CONFIGURE_SCRIPTS =
TEST_SCRIPTS      = 
INSTALL_SCRIPTS   = ruby-augeas

sitelibdir = $(shell ruby -rrbconfig -e "puts RbConfig::CONFIG['sitelibdir']")
sitearchdir = $(shell ruby -rrbconfig -e "puts RbConfig::CONFIG['sitearchdir']")

include gar/category.mk

install-ruby-augeas:
	ginstall -m 755 -d $(DESTDIR)
	ginstall -m 755 -d $(DESTDIR)$(sitearchdir)
	ginstall -m 755 -d $(DESTDIR)$(sitelibdir)
	cd $(WORKSRC) && rake build
	ginstall -m 755 $(WORKSRC)/lib/augeas.rb $(DESTDIR)$(sitelibdir)
	ginstall -m 755 $(WORKSRC)/ext/augeas/_augeas.so $(DESTDIR)$(sitearchdir)
	@$(MAKECOOKIE)
