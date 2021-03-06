# 
# Copyright (C) 2008 David Cooper <dave@kupesoft.com>
# Copyright (C) 2006-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=sox
PKG_VERSION:=14.4.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=@SF/sox
PKG_MD5SUM:=813807c6d6fcd4b0af5c995e61f46687

PKG_LICENSE:=LGPLv2.1 GPLv2
PKG_LICENSE_FILES:=COPYING LICENSE.LGPL LICENSE.GPL

PKG_FIXUP:=autoreconf

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

TARGET_LDFLAGS+= \
	-Wl,-rpath-link=$(STAGING_DIR)/usr/lib

# NOTE: libflac is out of date for 8.09's freeze and can't be compiled in

define Package/sox
  SECTION:=sound
  CATEGORY:=Sound
  DEPENDS:=+BUILD_PATENTED:lame-lib +BUILD_PATENTED:libmad +BUILD_PATENTED:libid3tag +libvorbis +libvorbisidec +libgsm +alsa-lib +libsndfile +libflac +ffmpeg +libmagic +libpng
  TITLE:=Sox is a general purpose sound converter/player/recorder
  URL:=http://sox.sourceforge.net/
  MAINTAINER:=Hamish Guthrie <hcg@openwrt.org>
endef

define Package/sox/description
	SoX is a command line utility that can convert various formats
	of computer audio files in to other formats. It can also apply
	various effects to these sound files during the conversion.
	As an added bonus, SoX can play and record audio files on
	several unix-style platforms.
endef

define Build/Configure
	$(call Build/Configure/Default, \
		--disable-external-gsm \
		--disable-oss \
		--enable-alsa \
		--disable-libao \
		--without-libltdl \
		--with-ogg \
		--with-flac \
		--with-ffmpeg \
		--without-amr-wb \
		--without-amr-nb \
		--without-samplerate \
		--without-ladspa \
		--$(if $(CONFIG_BUILD_PATENTED),with-mad,without-mad) \
		--$(if $(CONFIG_BUILD_PATENTED),with-lame,without-lame) \
		--$(if $(CONFIG_BUILD_PATENTED),with-id3tag,without-id3tag) \
	)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/include \
		$(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/libsox.{a,so*,la} \
		$(1)/usr/lib/
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/* \
		$(1)/usr/lib/pkgconfig/
endef

define Package/sox/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/{play,rec,sox} $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libsox.so* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,sox))

