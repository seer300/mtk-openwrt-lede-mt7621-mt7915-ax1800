#
# L2oGRE
#

include $(TOPDIR)/rules.mk

PKG_NAME:=l2ogre
PKG_VERSION:=1
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)
PKG_CONFIG_DEPENDS:=

include $(INCLUDE_DIR)/package.mk

define Package/l2ogre
  SECTION:=MTK Properties
  CATEGORY:=MTK Properties
  SUBMENU:=Applications
  TITLE:=L2oGRE
  VERSION:=$(PKG_RELEASE)-$(REVISION)
  DEPENDS:=+ip-full
endef

define Package/l2ogre/description
  L2oGRE
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
endef

define Build/Compile/Default
endef

Build/Compile = $(Build/Compile/Default)

define Package/l2ogre/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/l2ogre.init $(1)/etc/init.d/l2ogre
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/l2ogre.conf $(1)/etc/config/l2ogre
endef


$(eval $(call BuildPackage,l2ogre))

