#
# Copyright (C) 2006-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ntpclient
PKG_VERSION:=2007_365
PKG_RELEASE:=4

PKG_SOURCE:=$(PKG_NAME)_$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=$(SVN_REPOSITORY_ROOT)/openwrt/dl
PKG_SOURCE_PROTO:=svn_pkg
PKG_SOURCE_VERSION:=HEAD
PKG_MD5SUM:=cb98711f11769cdd22fc592844cef414

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-2007

ifeq ($(CONFIG_UBOOT_TARGET),$(filter $(CONFIG_UBOOT_TARGET),"ruby" "wap5806"))
PATCH_DIR:=patches-ruby
endif

include $(INCLUDE_DIR)/package.mk

define Package/ntpclient
  SUBMENU:=Time Synchronization
  SECTION:=net
  CATEGORY:=Network
  TITLE:=NTP (Network Time Protocol) client
  URL:=http://doolittle.icarus.com/ntpclient/
  DEPENDS:=+librt
endef

define Package/ntpclient/description
	NTP client for setting system time from NTP servers.
endef

define Package/ntpclient/conffiles
/etc/config/ntpclient
endef

MAKE_FLAGS += \
	all adjtimex

define Package/ntpclient/install
	$(INSTALL_DIR) $(1)/etc/hotplug.d/iface
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/ntpclient.config $(1)/etc/config/ntpclient
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ntpclient $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/adjtimex $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/rate.awk $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/init.d
ifeq ($(CONFIG_UBOOT_ZYXEL_TARGET),"emg2926")
	$(INSTALL_BIN) ./files/time_daemon_EMG2926VT.init $(1)/etc/init.d/time_daemon
else ifeq ($(CONFIG_PACKAGE_auto_detect_timezone),y)
	$(INSTALL_BIN) ./files/time_daemon_auto.init $(1)/etc/init.d/time_daemon
else
	$(INSTALL_BIN) ./files/time_daemon.init $(1)/etc/init.d/time_daemon
endif
	$(INSTALL_DIR) $(1)/sbin
ifeq ($(CONFIG_UBOOT_ZYXEL_TARGET),"emg2926")
	$(INSTALL_BIN) ./files/time_daemon_EMG2926VT $(1)/sbin/time_daemon
else
	$(INSTALL_BIN) ./files/time_daemon $(1)/sbin/time_daemon
endif
ifeq ($(CONFIG_UBOOT_ZYXEL_TARGET),"emg2926aavk")
	$(INSTALL_DATA) ./files/ntpclient.hotplug_emg2926aavk $(1)/etc/hotplug.d/iface/20-ntpclient
else ifeq ($(CONFIG_UBOOT_ZYXEL_TARGET),"emg3425aayj")
	$(INSTALL_DATA) ./files/ntpclient.hotplug_emg2926aavk $(1)/etc/hotplug.d/iface/20-ntpclient
else
	$(INSTALL_DATA) ./files/ntpclient.hotplug $(1)/etc/hotplug.d/iface/20-ntpclient
endif

endef

$(eval $(call BuildPackage,ntpclient))
