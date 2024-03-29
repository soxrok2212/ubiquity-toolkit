PKG_NAME:=busybox
PKG_VERSION:=1.36.1
PKG:=$(PKG_NAME)-$(PKG_VERSION)
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG).tar.bz2
PKG_SOURCE_URL:=https://www.busybox.net/downloads/$(PKG_SOURCE)
PKG_HASH:=

ARCH ?= x86_64-linux-musl
CFLAGS := 
LDFLAGS := --static
CC := $(ARCH)-gcc
CXX := $(ARCH)-g++
STRIP := $(ARCH)-strip
CONFIGURE_FLAGS := CROSS_COMPILE="$(ARCH)-"
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
else ifeq ($(ARCH), mipsel-linux-musl)
    TARGET := linux-generic32
    SUFFIX := mips32el
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
	@cd $(ARCH) && wget -q $(PKG_SOURCE_URL)
	@cd $(ARCH) && tar xf $(PKG_SOURCE)

compile: download
	@echo "I: building $(PKG_NAME)"
	@cp .config $(ARCH)/$(PKG)
	@cd $(ARCH)/$(PKG) && LDFLAGS="$(LDFLAGS)" make -j $(MAKEFLAGS) $(CONFIGURE_FLAGS)

archive: compile
	@echo "I: archiving"
	@mkdir $(ARCH)/bin
	@cp $(ARCH)/$(PKG)/busybox $(ARCH)/bin/busybox-$(SUFFIX)
	@$(STRIP) $(ARCH)/bin/*

clean:
	@echo "I: cleaning up"
	@rm -rf *linux*


