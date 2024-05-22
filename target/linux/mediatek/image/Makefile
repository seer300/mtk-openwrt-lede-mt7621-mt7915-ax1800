#
# Copyright (C) 2012-2015 OpenWrt.org
# Copyright (C) 2016-2017 LEDE project
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/image.mk

# build dtb
define Build/dtb
        $(call Image/BuildDTB,$(DEVICE_DTS_DIR)/$(DEVICE_DTS).dts,$(DEVICE_DTS_DIR)/$(DEVICE_DTS).dtb)
	$(CP) $(DEVICE_DTS_DIR)/$(DEVICE_DTS).dtb $(BIN_DIR)/
endef

# build squashfs-hashed
define Build/squashfs-hashed
	$(CP) $(KDIR)/root.squashfs $(KDIR)/root.squashfs-hashed
	$(TOPDIR)/scripts/make-squashfs-hashed.sh $(KDIR)/root.squashfs-hashed $(STAGING_DIR_HOST) $(TOPDIR) $(KDIR)/root.squashfs-hashed-summary
	cat $(KDIR)/root.squashfs-hashed-summary | \
		${TOPDIR}/scripts/prepare-dm-verity-uboot-script.sh "$(HASHED_BOOT_DEVICE)" "$(BASIC_KERNEL_CMDLINE)" \
			> $(KDIR)/$(DEVICE_NAME)-u-boot-script.sh
endef
DEFAULT_DEVICE_VARS += HASHED_BOOT_DEVICE BASIC_KERNEL_CMDLINE

# append squashfs-hashed
define Build/append-squashfs-hashed
	cat $(KDIR)/root.squashfs-hashed >> $@
endef

# build fit-ar-ver
get_fit_ar_ver = \
  $(if $(wildcard $(2)),$(shell rm -rf $(2))) \
  $(if $(wildcard $(1)),$(info $(shell $(STAGING_DIR_HOST)/bin/ar-table create_ar_conf $(1) $(2)))) \
  $(if $(wildcard $(2)),$(eval include $(2))) \
  $(if $(FIT_AR_VER),$(info FIT_AR_VER = $(FIT_AR_VER)))

define Build/fit-ar-ver
	$(call get_fit_ar_ver,$(AR_TABLE_XML),$(AUTO_AR_CONF))
endef

# build signed fit
define Build/fit-sign
	$(TOPDIR)/scripts/mkits.sh \
		-D $(DEVICE_NAME) -o $@.its -k $@ \
		-d $(word 2,$(1)) -C $(word 1,$(1)) \
		-a $(KERNEL_LOADADDR) -e $(if $(KERNEL_ENTRY),$(KERNEL_ENTRY),$(KERNEL_LOADADDR)) \
		-A $(LINUX_KARCH) -v $(LINUX_VERSION) \
		-s $(KDIR)/$(DEVICE_NAME)-u-boot-script.sh \
		$(if $(FIT_KEY_NAME),-S $(FIT_KEY_NAME)) \
		$(if $(FIT_AR_VER),-r $(FIT_AR_VER))
	PATH=$(LINUX_DIR)/scripts/dtc:$(PATH) mkimage -f $@.its \
		$(if $(FIT_KEY_DIR),-k $(FIT_KEY_DIR)) \
		-r $@.new
	@mv $@.new $@
endef
DEFAULT_DEVICE_VARS += FIT_KEY_NAME FIT_KEY_DIR

# default all platform image(fit) build
define Device/Default
  PROFILES = Default $$(DEVICE_NAME)
  KERNEL_NAME := Image
  FIT_KEY_DIR :=
  FIT_KEY_NAME :=
  FILESYSTEMS := squashfs
  DEVICE_DTS_DIR := $(DTS_DIR)
  KERNEL = dtb | kernel-bin | lzma | fit lzma $$(DEVICE_DTS_DIR)/$$(DEVICE_DTS).dtb
  IMAGES := sysupgrade.bin
  IMAGE/sysupgrade.bin := append-kernel | pad-to 128k | append-rootfs | pad-rootfs | append-metadata
endef
DEFAULT_DEVICE_VARS += AR_TABLE_XML AUTO_AR_CONF

include $(SUBTARGET).mk

$(eval $(call BuildImage))

