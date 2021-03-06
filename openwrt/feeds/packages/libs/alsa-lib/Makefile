#
# Copyright (C) 2006-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=alsa-lib
PKG_VERSION:=1.1.4.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=ftp://ftp.alsa-project.org/pub/lib/ \
		http://distfiles.gentoo.org/distfiles/

PKG_HASH:=91bb870c14d1c7c269213285eeed874fa3d28112077db061a3af8010d0885b76
PKG_MAINTAINER:=Ted Hess <thess@kitschensync.net>, \
		Peter Wagner <tripolar@gmx.at>

PKG_LICENSE:=LGPLv2.1 GPLv2
PKG_LICENSE_FILES:=COPYING aserver/COPYING

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1
PKG_USE_MIPS16:=0
PKG_CHECK_FORMAT_SECURITY:=0

include $(INCLUDE_DIR)/package.mk

define Package/alsa-lib
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=ALSA (Advanced Linux Sound Architecture) library
  URL:=http://www.alsa-project.org/
  DEPENDS:=@AUDIO_SUPPORT +kmod-sound-core +libpthread +librt
endef

define Package/alsa-lib/description
 This is the library package for alsa, needed by some userspace programs.
 You must have enabled the ALSA support in the kernel.
endef

TARGET_CFLAGS += $(FPIC)

CONFIGURE_ARGS+= \
		--disable-python \
		--disable-debug \
		--without-debug \
		$(SOFT_FLOAT_CONFIG_OPTION) \
		--with-versioned=no

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/include/alsa \
		$(1)/usr/include/

	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/libasound.{la,so*} \
		$(1)/usr/lib/
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/alsa.pc \
		$(1)/usr/lib/pkgconfig/

	$(INSTALL_DIR) $(1)/usr/share/aclocal
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/share/aclocal/alsa.m4 \
		$(1)/usr/share/aclocal/
endef

define Package/alsa-lib/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/libasound.so.* \
		$(1)/usr/lib/

	$(INSTALL_DIR) $(1)/usr/share/alsa/{cards,pcm}
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/share/alsa/alsa.conf \
		$(1)/usr/share/alsa/
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/share/alsa/pcm/* \
		$(1)/usr/share/alsa/pcm/
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/share/alsa/cards/* \
		$(1)/usr/share/alsa/cards/

ifeq ($(CONFIG_TARGET_amlogic_a113), y)
	$(CP) \
		./files/alsa.conf $(1)/usr/share/alsa/

	$(INSTALL_DIR) $(1)/etc
  ifeq ($(CONFIG_subtarget_version),"-rokidme")
	$(CP) ./files/asound_rokidme.conf $(1)/etc/asound.conf
	$(CP) ./files/alsa_me.conf $(1)/usr/share/alsa/alsa.conf
  else ifeq ($(CONFIG_subtarget_version),"-rokidmini")
	$(CP) ./files/asound_mini.conf $(1)/etc/asound.conf
  else ifeq ($(CONFIG_subtarget_version),"-tuyamini")
	$(CP) ./files/asound_mini.conf $(1)/etc/asound.conf
  else
	$(CP) ./files/asound.conf $(1)/etc/asound.conf
  endif

endif

ifeq ($(CONFIG_TARGET_quanzhi), y)
	$(INSTALL_DIR) $(1)/etc
	$(CP) ./files/asound_quanzhi.conf $(1)/etc/asound.conf
endif

endef

$(eval $(call BuildPackage,alsa-lib))
