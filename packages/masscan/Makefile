PKG_NAME:=masscan
PKG_VERSION:=1.3.2
PKG:=$(PKG_NAME)-$(PKG_VERSION)
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG).tgz
PKG_SOURCE_URL:=https://github.com/robertdavidgraham/masscan
PKG_HASH:=

FULL_PATH := $(shell dirname $(shell dirname $(shell pwd)))

ARCH ?= x86_64-linux-musl
CFLAGS := 
CC := $(ARCH)-gcc
CXX := $(ARCH)-g++
STRIP := $(ARCH)-strip
CONFIGURE_FLAGS := CFLAGS="-Os -DSTATICPCAP -static -L$(FULL_PATH)/libs/libpcap/$(ARCH)/libpcap-1.10.4 -I$(FULL_PATH)/libs/libpcap/$(ARCH)/libpcap-1.10.4"
MAKEFLAGS := -j $(shell nproc)

TARGET ?= linux-x86_64
SUFFIX ?= x64

.PHONY: all
all: compile archive

ifeq ($(ARCH), aarch64-linux-musl)
    TARGET := linux-aarch64
    SUFFIX := arm64
else ifeq ($(ARCH), arm-linux-musleabi)
    TARGET := linux-generic32
    SUFFIX := arm32
else ifeq ($(ARCH), armeb-linux-musleabi)
    TARGET := linux-generic32
    SUFFIX := arm32eb
else ifeq ($(ARCH), mips64-linux-musl)
    TARGET := linux-generic64
    SUFFIX := mips64
else ifeq ($(ARCH), mips-linux-musl)
    TARGET := linux-generic32
    SUFFIX := mips32
else ifeq ($(ARCH), mips64el-linux-musl)
    TARGET := linux-generic64
    SUFFIX := mips64el
    CONFIGURE_FLAGS := CFLAGS="-Os -DSTATICPCAP -static -L$(FULL_PATH)/libs/libpcap/$(ARCH)/libpcap-1.10.4 -I$(FULL_PATH)/libs/libpcap/$(ARCH)/libpcap-1.10.4 -fPIC"
else ifeq ($(ARCH), mipsel-linux-musl)
    TARGET := linux-generic32
    SUFFIX := mips32el
    CONFIGURE_FLAGS := CFLAGS="-Os -DSTATICPCAP -static -L$(FULL_PATH)/libs/libpcap/$(ARCH)/libpcap-1.10.4 -I$(FULL_PATH)/libs/libpcap/$(ARCH)/libpcap-1.10.4 -fPIC"
else ifeq ($(ARCH), riscv64-linux-musl)
    TARGET := linux-riscv64
    SUFFIX := riscv64
else ifeq ($(ARCH), i386-linux-musl)
    TARGET := linux-x86
    SUFFIX := x86
else ifeq ($(ARCH), x86_64-linux-musl)
    TARGET := linux-x86-64
    SUFFIX := x64
else ifeq ($(ARCH), powerpc-linux-musl)
    TARGET := linux-ppc
    SUFFIX := ppc32
else ifeq ($(ARCH), powerpc64-linux-musl)
    TARGET := linux-ppc64
    SUFFIX := ppc64
else ifeq ($(ARCH), powerpc64le-linux-musl)
    TARGET := linux-ppc64le
    SUFFIX := ppc64le
else
    $(error E: a valid arch must be specified)
endif

download:
	@mkdir $(ARCH)
	@echo "I: downloading $(PKG_SOURCE)"
	@cd $(ARCH) && git clone $(PKG_SOURCE_URL) ${PKG}

compile: download
	@echo "I: building $(PKG_NAME)"
	@cd $(ARCH)/$(PKG) && patch -p1 < ../../patches/masscan.patch
	@cd $(ARCH)/$(PKG) && CC="$(CC)" CXX="$(CXX)" make $(MAKEFLAGS) $(CONFIGURE_FLAGS)

archive: compile
	@echo "I: archiving"
	@mkdir $(ARCH)/bin
	@cp $(ARCH)/$(PKG)/bin/masscan $(ARCH)/bin/masscan-$(SUFFIX)
	@$(STRIP) $(ARCH)/bin/*

clean:
	@echo "I: cleaning up"
	@rm -rf *linux*


