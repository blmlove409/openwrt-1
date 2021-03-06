#
# Copyright (C) 2015 wongsyrone
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=DNSPing
PKG_VERSION:=0.1.1
PKG_RELEASE:=2

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/chengr28/DNSPing.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=39a1d502212cabc19c581e3d43165573c860195c
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
CMAKE_INSTALL:=1

PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_USE_MIPS16:=0
PKG_BUILD_PARALLEL:=1

PKG_MAINTAINER:=Chengr28 <chengr28@twitter>

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

TARGET_CFLAGS += $(FPIC)

CMAKE_SOURCE_DIR:=Source/DNSPing

CMAKE_OPTIONS += \
	-DPLATFORM_OPENWRT=ON

# Note: GCC 4.6 and 4.8 dont have complete C++11 support
#       Please use GCC 4.9 or higher to compile
define Package/DNSPing
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Ping with DNS requesting.
	URL:=https://github.com/chengr28/DNSPing
	DEPENDS:=+libstdcpp \
		@GCC_VERSION_4_6:BROKEN @GCC_VERSION_4_8:BROKEN
endef

define Package/DNSPing/config
	if PACKAGE_DNSPing
	config PACKAGE_DNSPing_advancedoptions
		bool "Use advanced compile options, see Makefile for details."
		default n
	endif
endef

define Package/DNSPing/description
A network util can ping with DNS request.
endef

# Some advanced compile flags for expert
ifneq ($(CONFIG_PACKAGE_DNSPing_advancedoptions),)
	# Try to reduce binary size
	TARGET_CFLAGS += -ffunction-sections -fdata-sections
	TARGET_LDFLAGS += -Wl,--gc-sections
	# Use Link time optimization
	TARGET_CFLAGS += -flto
	TARGET_LDFLAGS += -Wl,-flto
endif

define Package/DNSPing/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/DNSPing $(1)/usr/sbin/DNSPing
endef


$(eval $(call BuildPackage,DNSPing))