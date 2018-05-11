USE_SYSTEM_LIB ?= 0

BUILD_DIR = $(realpath rpm)
SPEC_PATH = $(BUILD_DIR)/SPECS/shadowsocks-libev.spec

HAS_GIT := $(shell command -v git 2> /dev/null)
ifndef HAS_GIT
deps:
	dnf -y install git
else
deps:
endif

export-git-version: deps
	$(eval GIT_VERSION_CHUNKS=$(shell scripts/git-version))

	$(eval GIT_VERSION=$(word 1,$(GIT_VERSION_CHUNKS)))
	$(eval GIT_COMMITS=$(word 2,$(GIT_VERSION_CHUNKS)))
	$(eval GIT_SHA1=$(word 3,$(GIT_VERSION_CHUNKS)))

	$(eval GIT_ARCHIVE=shadowsocks-libev-$(GIT_VERSION))

git-archive: export-git-version
	scripts/git-archive -o $(BUILD_DIR)/SOURCES/ -n $(GIT_ARCHIVE)

$(SPEC_PATH): $(SPEC_PATH).in export-git-version
	sed -e "s/@VERSION@/$(GIT_VERSION)/" \
	    -e "s/@RELEASE@/1.$(GIT_COMMITS).git$(GIT_SHA1)/" \
	    -e "s/@SOURCE@/$(GIT_ARCHIVE)/" \
	$< > $@

srpm: deps git-archive $(SPEC_PATH)
	rpmbuild -bs $(SPEC_PATH) \
	    --define "%_topdir $(BUILD_DIR)" \
	    --define "%_srcrpmdir $(outdir)" \
	    --define "%use_system_lib $(USE_SYSTEM_LIB)"

all: srpm

.PHONY: export-git-version git-archive deps srpm all
