SHELL = /bin/bash
.ONESHELL:
.SHELLFLAGS += -e

KLISH_VERSION = 2.1.4
PYTHONVER=3.7

KLISH_SRC = $(SONIC_CLI_ROOT)/klish-$(KLISH_VERSION)

SRC_REPLACEMENTS:=$(shell find patches -type f)
all : $(SRC_REPLACEMENTS)
	tar xzvf klish-$(KLISH_VERSION).tgz -C $(SONIC_CLI_ROOT)
	./patches/scripts/patchmake.sh -p VER=${KLISH_VERSION} TSP=${SONIC_CLI_ROOT} DSP=${CURDIR}/patches TWP=${SONIC_CLI_ROOT}

	cd ${KLISH_SRC} && sh autogen.sh && ./configure --with-libxml2=/usr --enable-debug=no LIBS='-l:libcurl-gnutls.so.4 -L/usr/lib/x86_64-linux-gnu -lpython3.7m -Wl,-rpath=/usr/lib/x86_64-linux-gnu -lcjson' CFLAGS='${CFLAGS} -g -I/usr/include/python3.7m' CPPFLAGS='${CPPFLAGS} -I/usr/include/cjson' && make

	mkdir -p $(SONIC_CLI_ROOT)/target/.libs
	cp $(CURDIR)/clish_start $(SONIC_CLI_ROOT)/target/.

	cp -r ${KLISH_SRC}/bin/.libs/clish   ${SONIC_CLI_ROOT}/target/.
	cp -r ${KLISH_SRC}/.libs/*.so* ${SONIC_CLI_ROOT}/target/.libs
	cp -r ${KLISH_SRC}/.libs/*.a   ${SONIC_CLI_ROOT}/target/.libs
	@echo "complete klish build"

.PHONY: clean
clean:
	rm -rf ${KLISH_SRC}
