TAREGT=iphone:latest:2.0
PLATFORM=Linux
export THEOS=/toolchain4/theos
export SDKBINPATH=/toolchain4/pre/bin
export SYSROOT=/toolchain4/sys50
export TARGET_CXX=clang -ccc-host-triple arm-apple-darwin9 
include $(THEOS)/makefiles/common.mk

BUNDLE_NAME = widgettest
widgettest_FILES = widgettestController.m
widgettest_INSTALL_PATH = /System/Library/WeeAppPlugins/
widgettest_FRAMEWORKS = UIKit CoreGraphics

IPHONEOSMINVERSION:=50000
ADDITIONAL_CFLAGS = -D__IPHONE_OS_VERSION_MIN_REQUIRED=$(IPHONEOSMINVERSION)

include $(THEOS_MAKE_PATH)/bundle.mk

after-install::
	install.exec "killall -9 SpringBoard"

after-stage::
	mkdir -p $(THEOS_STAGING_DIR)/DEBIAN
	cp control $(THEOS_STAGING_DIR)/DEBIAN/.

PACKAGEFOLDER=$(THEOS_STAGING_DIR)
PACKAGENAME=$(shell grep ^Package: ${PACKAGEFOLDER}/DEBIAN/control | cut -d ' ' -f 2)
PACKAGEVERSION=$(shell grep ^Version: ${PACKAGEFOLDER}/DEBIAN/control | cut -d ' ' -f 2)
ARCH=$(shell grep ^Architecture: ${PACKAGEFOLDER}/DEBIAN/control | cut -d ' ' -f 2)
_EXCLUDES ?= tmp _MTN .git .svn .DS_Store ._*
_EXCLUDE_COMMANDLINE := $(foreach exclude,$(_EXCLUDES),--exclude "$(exclude)")

deb:
	rm -f $(PACKAGENAME)_$(PACKAGEVERSION)_$(ARCH).deb $(PACKAGEFOLDER)/control.tar.gz $(PACKAGEFOLDER)/data.tar.gz
	rm -f $(PACKAGEFOLDER)/tmp/*.gz
	mkdir -p $(PACKAGEFOLDER)/tmp
	echo "2.0" > $(PACKAGEFOLDER)/tmp/debian-binary
	cd $(PACKAGEFOLDER)/DEBIAN; tar -czf ../tmp/control.tar.gz ./
	cd $(PACKAGEFOLDER); tar -czf ./tmp/data.tar.gz $(_EXCLUDE_COMMANDLINE) ./
	cd $(PACKAGEFOLDER)/tmp;  ar -rv ../../$(PACKAGENAME)_$(PACKAGEVERSION)_$(ARCH).deb ./debian-binary ./control.tar.gz ./data.tar.gz
	rm -fr $(PACKAGEFOLDER)/tmp/*.gz

