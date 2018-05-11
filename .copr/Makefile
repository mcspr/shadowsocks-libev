GIT_ARCHIVE = $(shell scripts/git-archive -n)

GIT_VERSION = $(shell scripts/git-version -b)
GIT_REVISION = $(shell scripts/git-version -r)
ifeq ($(GIT_REVISION),)
    GIT_RELEASE = 1
else
    GIT_RELEASE = 1.$(GIT_REVISION)
endif

USE_SYSTEM_LIB ?= 0

BUILD_DIR = rpm
SPEC_PATH = $(BUILD_DIR)/SPECS/shadowsocks-libev.spec

deps:
	dnf -y install git rpm-build

rpm/SOURCES/$(GIT_ARCHIVE):
	scripts/git-archive -a
	cp $(GIT_ARCHIVE) rpm/SOURCES/

$(SPEC_PATH): $(SPEC_PATH).in
	sed -e "s/@VERSION@/$(GIT_VERSION)/" \
	    -e "s/@RELEASE@/$(GIT_RELEASE)/" \
	    -e "s/@SOURCE@/$(GIT_ARCHIVE)/" \
	$< > $@

srpm: deps $(SPEC_PATH) rpm/SOURCES/$(GIT_ARCHIVE)
	rpmbuild -bs $(SPEC_PATH) \
	    --define "%_topdir $(BUILD_DIR)" \
	    --define "%use_system_lib $(USE_SYSTEM_LIB)"

.PHONY: deps srpm
