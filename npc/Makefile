# Copyright (C) 2024-2025 Duan <duan@d-jy.net>

include $(TOPDIR)/rules.mk

PKG_NAME:=npc
PKG_VERSION:=0.35.0
PKG_RELEASE:=1
PKG_LICENSE:=Apache-2.0
PKG_MAINTAINER:=Duan <duan@d-jy.net>
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

NPS_URL=https://github.com/djylb/nps

NPC_ARCH:=
ifeq ($(ARCH),x86_64)
	NPC_ARCH:=amd64
endif
ifeq ($(ARCH),i386)
	NPC_ARCH:=386
endif
ifeq ($(ARCH),aarch64)
	NPC_ARCH:=arm64
endif
ifeq ($(findstring armv8,$(BOARD)),armv8)
	NPC_ARCH:=arm64
endif
ifeq ($(ARCH),arm)
	ifeq ($(BOARD),bcm53xx)
		NPC_ARCH:=arm_v6
	else ifeq ($(BOARD),kirkwood)
		NPC_ARCH:=arm_v5
	else
		NPC_ARCH:=arm_v7
	endif
endif
ifeq ($(filter mipsel,$(ARCH)),mipsel)
	NPC_ARCH:=mipsle_softfloat
endif
ifeq ($(filter mips64el,$(ARCH)),mips64el)
	NPC_ARCH:=mips64le
endif
ifeq ($(filter mips64,$(ARCH)),mips64)
	NPC_ARCH:=mips64
endif
ifeq ($(filter mips,$(ARCH)),mips)
	NPC_ARCH:=mips_softfloat
endif
ifeq ($(filter loongarch64,$(ARCH)),loongarch64)
	NPC_ARCH:=loong64
endif
ifeq ($(filter riscv64,$(ARCH)),riscv64)
	NPC_ARCH:=riscv64
endif

PKG_SOURCE:=linux_$(NPC_ARCH)_client.tar.gz
PKG_SOURCE_URL:=$(NPS_URL)/releases/download/v$(PKG_VERSION)/

# === NPC_HASH_TABLE_START ===
NPC_HASHES:= \
  amd64=3910b86660a919ac78c7b1b5b6f6e7a8bcd9dbf1f341396d1471157e7fefc815 \
  386=db5a41dc3344e1ab098518c66863dc9ea196b2bf22a0ca2de3ec7f577865b44d \
  arm64=f10fc69e45b64efa9c01acc5ee3a43d36e0ace70c058dca6a2ca72635421501b \
  arm_v7=f286c286bc0cbdf3cce3ec96d0fd184585595165e22c9fa7c4e00dfc5269bbf5 \
  arm_v6=7efee2c6706395c71ebdfb6ebde93d145d9eb5faab839c7acb20bbf20cce8f0e \
  arm_v5=f2f9fe57c74cec5ff5221f7ebaa7a897276fa46bd3f933e91ffad15374b1fdcc \
  mips_softfloat=379b94d09a7fa2c1f0ee04c884549bf5048ee5023de91fc8398eff6d8b4b5212 \
  mipsle_softfloat=f600e27c16ac15a80e6512f380e1f1d93611b169fffd70a33aea157c0f01a338 \
  mips64=3fa98f24b87f110702150e82ea2e1e035f7d9453c63e9ebf931784bd2754a8c2 \
  mips64le=22f9131b650eb03e3bc16a969fa1d6e357c5f658353802de22525d2d305b7a49 \
  loong64=b748d4ac0613d0a16c06185895e76da7883abad345f3e6db1eea972df63ae57b \
  riscv64=03d5ed76ddc09e8f896c3dc4740ac2307496b5b016c4f9f8d8131a4e95ce7106
# === NPC_HASH_TABLE_END ===

PKG_HASH:=$(strip $(foreach pair,$(NPC_HASHES), $(if $(findstring $(NPC_ARCH),$(pair)),$(word 2, $(subst =, ,$(pair))))))

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	TITLE:=NPC Client (precompiled for $(NPC_ARCH))
	URL:=$(NPS_URL)
endef

define Package/$(PKG_NAME)/description
NPC is a fast reverse proxy client to expose local servers through NAT/firewall.
This package uses the precompiled binary for the $(NPC_ARCH) architecture.
endef

define Build/Compile
	mkdir -p $(PKG_BUILD_DIR)
	tar -xzf $(DL_DIR)/$(PKG_SOURCE) -C $(PKG_BUILD_DIR)
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(PKG_NAME) $(1)/usr/bin/$(PKG_NAME)
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
