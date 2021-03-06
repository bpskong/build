NAME     = alsa-lib
TITLE    = alsa-lib
APP_ID   = org.webosinternals.toolchain.${NAME}
SRC_VER  = 1.0.18
VERSION  = 1.0.18-2
MAINTAINER = WebOS Internals <support@webos-internals.org>

SRC_TGZ = http://downloads.help.palm.com/opensource/2.1.2/${NAME}-${SRC_VER}.tgz

.PHONY: package
package: build/.built-${VERSION}

include ../../support/download.mk

.PHONY: stage
stage: build/.staged-${VERSION}

build/.staged-${VERSION}: build/armv7.staged-${VERSION} build/armv6.staged-${VERSION} build/i686.staged-${VERSION}
	touch $@

build/%.staged-${VERSION} : build/%.built-${VERSION}
	mkdir -p ../../staging/$*/usr/include
	cp -rp build/$*/usr/include/* ../../staging/$*/usr/include/
	mkdir -p ../../staging/$*/usr/lib
	cp -rp build/$*/usr/lib/* ../../staging/$*/usr/lib/
	touch $@

.PHONY: unpack
unpack: build/.unpacked-${VERSION}

build/.unpacked-${VERSION}: ${DL_DIR}/${NAME}-${VERSION}.tar.gz
	rm -rf build
	mkdir -p build/armv6 build/armv7 build/i686
	tar -C build -z -x -f $<
	mv build/${NAME}-${SRC_VER} build/src
	${MAKE} SRC_FILE=http://downloads.help.palm.com/opensource/2.1.2/${NAME}-${SRC_VER}-patch.gz \
		LOCAL_FILE=${NAME}-${VERSION}-patch.gz download
	#cd build/src; cat ../../${DL_DIR}/${NAME}-${VERSION}-patch.gz | gunzip | patch -p0
	touch $@

.PHONY: build
build: build/.built-${VERSION}

build/.built-${VERSION}: build/armv7.built-${VERSION} build/armv6.built-${VERSION} build/i686.built-${VERSION}
	touch $@

include ../../support/cross-compile.mk

build/%.built-${VERSION}: build/.unpacked-${VERSION}
	rm -rf build/$*
	( cd build/src ; \
	  CC=${CROSS_COMPILE_$*}gcc \
	  CFLAGS="-D_GNU_SOURCE -I${STAGING_DIR_$*}/usr/include -I${STAGING_DIR_$*}/usr/include/gdbm" \
	  LIBLTDL=${STAGING_DIR_$*}/usr/lib \
	  LDFLAGS=-L${STAGING_DIR_$*}/usr/lib \
	  ./configure \
	  	--host=${CONFIGURE_HOST_$*} \
		--enable-ltdl-install=no \
		--disable-static \
		--disable-alisp \
		--disable-mixer \
		--disable-pcm \
		--disable-old-symbols \
		--disable-python ; \
	  ${MAKE} clean all )
	mkdir -p build/$*/usr/include/alsa
	install -m 644 build/src/include/*.h build/$*/usr/include/alsa/
	mkdir -p build/$*/usr/lib
	install -m 755 build/src/src/.libs/*.so* build/$*/usr/lib/
	touch $@

clobber::
	rm -rf build
