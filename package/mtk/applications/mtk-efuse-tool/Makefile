#
# MTK Property Software.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=mtk-efuse-tool
PKG_RELEASE:=1

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/kernel.mk

define Package/mtk-efuse-tool
  SECTION:=MTK Properties
  CATEGORY:=MTK Properties
  TITLE:=mtk efuse tool
  SUBMENU:=Applications
  DEPENDS:=+kmod-mtk-smc @TARGET_mediatek_mt7622
endef

define Build/Compile
endef

define Package/mtk-efuse-tool/description
  efuse tool to read/write efuse
endef

define Package/mtk-efuse-tool/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) ./files/mtk-efuse-tool.sh $(1)/usr/bin
endef

$(eval $(call BuildPackage,mtk-efuse-tool))

