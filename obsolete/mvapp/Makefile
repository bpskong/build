NAME     = mvapp
APP_ID   = org.webosinternals.${NAME}
VERSION  = 0.4.2
HOMEPAGE = http://www.webos-internals.org/wiki/Solution_to_App_Catalog_Installation_Limit
MAINTAINER = xorg <http://forums.precentral.net/members/xorg.html>
TITLE = MvApp
TYPE = Linux Application
CATEGORY = Utilities
ICON	= http://www.webos-internals.org/images/6/6b/Icon_MvApp.png
DESCRIPTION = A command-line script to allow moving applications to the larger /media/internal storage area.
CHANGELOG = 0.4.2: Added a new 'migrate' command and an alternate 'postupd2' command.<br>0.4.1: Added backup function to prep for webos updates.<br>0.3.6: Now checks for abandoned links and checks if the ipkg wrapper is installed. The doctor is renamed to diag, and added update preware and terminal apps.<br>0.3.4: Now has a 'search' function that makes it easier to move apps by partial app name.<br>0.3.2: Reporting improvements, cleaner launching of email,creates shortcut link<br>0.3.0: Now links email/attachments. New diagnostic doctor function.<br>0.2.5: Improved screen formatting for Terminal app, added shortcuts for commands, added /var usage size reporting<br>0.2.4: turned off TAR backups by default, added 'mvapp info', added two hidden functions (see mvapp homepage for details), 'mvapp ftr' - force tar restore, 'mvapp rtb' - remove tar backups
SCREENSHOTS = []

SRC_GIT = git://gitorious.org/webos-internals/mvapp.git

.PHONY: package
package: ipkgs/${APP_ID}_${VERSION}_all.ipk
include ../../support/package.mk

include ../../support/download.mk

.PHONY: unpack
unpack: build/.unpacked

build/.unpacked: ${DL_DIR}/${NAME}-${VERSION}.tar.gz
	rm -rf build
	mkdir -p build
	mkdir -p build/src
	tar -C build/src -xf ${DL_DIR}/${NAME}-${VERSION}.tar.gz
	touch $@

.PHONY: build
build: build/.built-${VERSION}

build/.built-${VERSION}: build/.unpacked
	rm -rf build/all
	mkdir -p build/all/usr/palm/applications/${APP_ID}
	install -m 755 build/src/mvapp build/all/usr/palm/applications/${APP_ID}/
	chmod ugo+rx build/all/usr/palm/applications/${APP_ID}/*
	touch $@

build/all/CONTROL/prerm: build/.unpacked
	rm -f $@
	mkdir -p build/all/CONTROL
	install -m 0775 files/prerm build/all/CONTROL
	chmod ugo+x $@

build/all/CONTROL/postinst: build/.unpacked
	rm -f $@
	mkdir -p build/all/CONTROL
	install -m 0775 files/postinst build/all/CONTROL
	chmod ugo+x $@

.PHONY: clobber
clobber::
	rm -rf build
