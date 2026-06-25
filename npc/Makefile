# Copyright (C) 2024-2025 Duan <duan@d-jy.net>

include $(TOPDIR)/rules.mk

PKG_NAME:=npc
PKG_VERSION:=0.35.0
PKG_RELEASE:=1
PKG_LICENSE:=Apache-2.0
PKG_MAINTAINER:=Duan <duan@d-jy.net>
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

NPS_URL=https://github.com/nue-mic/nps

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
  amd64=29dafff63fb131ec323b5adbc6fc298a5f80905a33bfec932b372e21591ab034 \
  386=6070ab0692354569a9afa9375459602a49aee5e2b536b6f4a0d6100a0941bb26 \
  arm64=3bd5648345aaf87be799904f7fb32a4ddc5660c5483569ed82283e6f08caa8e1 \
  arm_v7=5cc6ea219d12b25bf2a570b73580e578a23498509b23429fb97ecdaa25d09f83 \
  arm_v6=94563047c721bd2ff5cbad74c06b1d0f045464e8a4f755caded9954d9b4c6c49 \
  arm_v5=e602d6400291f389231a2b6d78a1989d779779f424a2324473a41fcb56ed76cd \
  mips_softfloat=74408dad4f8f612f9d251277a3a1225e84e05c5483ad31928a8c92dfcd72870d \
  mipsle_softfloat=1a890519ef07f105ab86936831f89873253dce1b9c0558a0858f26d92c78ca7a \
  mips64=6dfaa85b4a4cb088bcfc78c45d28e3170a680fdf1f526b5cf71d4fcff12a736f \
  mips64le=2d4868a9e8ad013fb075f7d2d20df97bdc4718ddc6f7e1ef6c2cab0c9dc21215 \
  loong64=2ef3d3bbb1b4f7a2ab404aaf20a9510be6935943186dbe95e005128e39c13777 \
  riscv64=5220dcc405583ef78218b4ccfa508a99a2418b71de899b90389e97b95bd3f94f
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
