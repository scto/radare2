include ../../../global.mk
F=
F+= linux-x86-32
F+= linux-x86-64
F+= darwin-x86-64
F+= windows-x86-32
F+= windows-x86-64

include $(TOP)/config-user.mk
HOST_CC?=gcc

F_SDB=$(addsuffix .sdb,$F)

SDBPATH=$(LTOP)/../shlr/sdb/
ifeq ($(BUILD_OS),windows)
BUILD_EXT_EXE=.exe
else
BUILD_EXT_EXE=
endif
SDB=$(SDBPATH)/sdb${BUILD_EXT_EXE}

all: ${SDB}
	@$(MAKE) compile

compile: ${F_SDB}

linux-x86-32.sdb: linux-x86-32

%.sdb:%
ifneq ($(SILENT),)
	@echo SDB $<
	@rm -f $<.sdb
	@cat $< | ${SDB} $<.sdb =
	@test -f $<.sdb
else
	rm -f $<.sdb
	cat $< | ${SDB} $<.sdb =
	test -f $<.sdb
endif

clean:
	rm -f *.sdb

${SDB}:
	@echo "Cannot find ${SDB}"
	@false

.PHONY: all clean install install-symlink symstall

install: ${F_SDB}
	-rm -rf "${DESTDIR}${LIBDIR}/radare2/${VERSION}/fcnsign"
	mkdir -p "${DESTDIR}${LIBDIR}/radare2/${VERSION}/fcnsign"
	cp -f *.sdb "${DESTDIR}${LIBDIR}/radare2/${VERSION}/fcnsign"

CWD=$(shell pwd)
symstall install-symlink: ${F_SDB}
	mkdir -p "${DESTDIR}${LIBDIR}/radare2/${VERSION}/fcnsign"
	for FILE in *.sdb ; do ln -fs "${CWD}/$$FILE" "${DESTDIR}${LIBDIR}/radare2/${VERSION}/fcnsign/$$FILE" ; done

uninstall:
	rm -rf "${DESTDIR}${LIBDIR}/radare2/fcnsign"
