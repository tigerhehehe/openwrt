include $(TOPDIR)/rules.mk

PKG_NAME:=ustream-ssl
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL=$(PROJECT_GIT)/project/ustream-ssl.git
PKG_SOURCE_DATE:=2024-04-07
PKG_SOURCE_VERSION:=d61493a4420417cbf9931ffee8c862faf04f2967
PKG_MIRROR_HASH:=a420d775ad4928836e33e0d9423486fe4904555dbbfff283cd96370a49cf9659
CMAKE_INSTALL:=1

PKG_LICENSE:=ISC
PKG_LICENSE_FILES:=

PKG_MAINTAINER:=Felix Fietkau <nbd@nbd.name>

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/libustream/default
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=ustream SSL Library
  DEPENDS:=+libubox
  ABI_VERSION:=20201210
endef

define Package/libustream-openssl
  $(Package/libustream/default)
  TITLE += (openssl)
  DEPENDS += +PACKAGE_libustream-openssl:libopenssl
  VARIANT:=openssl
endef

define Package/libustream-wolfssl
  $(Package/libustream/default)
  TITLE += (wolfssl)
  DEPENDS += +PACKAGE_libustream-wolfssl:libwolfssl
  CONFLICTS := libustream-openssl
  VARIANT:=wolfssl
endef

define Package/libustream-mbedtls
  $(Package/libustream/default)
  TITLE += (mbedtls)
  DEPENDS += +PACKAGE_libustream-mbedtls:libmbedtls
  CONFLICTS := libustream-openssl libustream-wolfssl
  VARIANT:=mbedtls
  DEFAULT_VARIANT:=1
endef

ifeq ($(BUILD_VARIANT),wolfssl)
  TARGET_CFLAGS += -I$(STAGING_DIR)/usr/include/wolfssl
  CMAKE_OPTIONS += -DWOLFSSL=on
endif
ifeq ($(BUILD_VARIANT),mbedtls)
  CMAKE_OPTIONS += -DMBEDTLS=on
endif

define Package/libustream/default/install
	$(INSTALL_DIR) $(1)/lib/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/libustream-ssl.so $(1)/lib/
endef

Package/libustream-openssl/install = $(Package/libustream/default/install)
Package/libustream-wolfssl/install = $(Package/libustream/default/install)
Package/libustream-mbedtls/install = $(Package/libustream/default/install)

$(eval $(call BuildPackage,libustream-mbedtls))
$(eval $(call BuildPackage,libustream-wolfssl))
$(eval $(call BuildPackage,libustream-openssl))
