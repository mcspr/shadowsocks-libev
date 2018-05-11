USE_SYSTEM_LIB ?= 0

BUILD_DIR = $(realpath rpm)
SPEC_PATH = $(BUILD_DIR)/SPECS/shadowsocks-libev.spec

deps:
	dnf -y install git

git-archive:
	scripts/git-archive -o $(BUILD_DIR)/SOURCES/ -a

$(SPEC_PATH): $(SPEC_PATH).in
	GIT_ARCHIVE=$(shell scripts/git-archive -n) ;\
	GIT_VERSION=$(shell scripts/git-version -b) ;\
	GIT_REVISION=$(shell scripts/git-version -r) ;\
	sed -e "s/@VERSION@/$$GIT_VERSION/" \
	    -e "s/@RELEASE@/$$GIT_REVISION/" \
	    -e "s/@SOURCE@/$$GIT_ARCHIVE/" \
	$< > $@

srpm: deps git-archive $(SPEC_PATH):
	rpmbuild -bs $(SPEC_PATH) \
	    --define "%_topdir $(BUILD_DIR)" \
	    --define "%use_system_lib $(USE_SYSTEM_LIB)"

.PHONY: deps srpm
