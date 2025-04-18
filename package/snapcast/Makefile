#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME := snapcast
PKG_VERSION := 0.29.0
PKG_RELEASE := $(PKG_SOURCE_VERSION)
PKG_USE_MIPS16 := 0

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/badaix/snapcast.git
PKG_SOURCE_VERSION:=v0.29.0
PKG_BUILD_DIR:=$(BUILD_DIR)/snapcast-$(PKG_VERSION)
PKG_MIRROR_HASH:=070704f549c6858d656a417064583ff635946909d002b6848ffcb40d18fffbf0

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/snapcast/Default
	SECTION := sound
	CATEGORY := Sound
	TITLE := Synchronous multiroom audio player
	DEPENDS := +libstdcpp +libavahi-client +libatomic +libogg +libflac +libopus +boost
	URL := https://github.com/badaix/snapcast
endef

define Package/snapcast/description/Default
	Synchronous audio player
endef

define Package/snapcast
	$(call Package/snapcast/Default)
	TITLE += packages
endef

define Package/snapcast/description
	$(call Package/snapcast/description/Default)
	Snapcast is a multi-room client-server audio player, 
	where all clients are time synchronized with the server 
	to play perfectly synced audio
endef

define Package/snapserver
	$(call Package/snapcast/Default)
	TITLE += Snapserver
	DEPENDS += +libvorbis +AUDIO_SUPPORT:alsa-lib +libvorbisidec +libsoxr +libflac +libopus +libexpat +libavahi-client
	HIDDEN := 1
endef

define Package/snapclient
	$(call Package/snapcast/Default)
	TILE += Snapclient
	DEPENDS += +AUDIO_SUPPORT:alsa-lib +AUDIO_SUPPORT:pulseaudio +libvorbisidec +libsoxr
	HIDDEN := 1
endef

define Package/snapserver/description
	$(call Package/snapcast/description/Default)
	Snapcast server
endef

define Package/snapclient/description
	$(call Package/snapcast/description/Default)
	Snapcast client
endef

define Package/snapcast/config
 	menu "Select Snapcast Options"
		depends on PACKAGE_snapcast
		comment "Choose server or client"
		config PACKAGE_snapserver
			prompt "Snapserver"
			help
				Snapcast server
			default n

		config PACKAGE_snapclient
			prompt "Snapclient"
			help
				Snapcast client
			default y
	endmenu
endef

ifeq ($(CONFIG_AUDIO_SUPPORT),y)
        TARGET_LDFLAGS += -Wl,-rpath-link=$(STAGING_DIR)/usr/lib/pulseaudio
endif

define Package/snapcast/Default/install
	true
endef

define Package/snapserver/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/snapserver $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/snapserver.init $(1)/etc/init.d/snapserver
	$(INSTALL_DIR) $(1)/etc/default
	$(INSTALL_BIN) ./files/snapserver.default $(1)/etc/default/snapserver
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/server/etc/snapserver.conf $(1)/etc/snapserver.conf
endef

define Package/snapclient/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/snapclient $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/snapclient.init $(1)/etc/init.d/snapclient
	$(INSTALL_DIR) $(1)/etc/default
	$(INSTALL_BIN) ./files/snapclient.default $(1)/etc/default/snapclient
endef

$(eval $(call BuildPackage,snapserver))
$(eval $(call BuildPackage,snapclient))
$(eval $(call BuildPackage,snapcast))
