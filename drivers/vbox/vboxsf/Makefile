#
# VirtualBox Guest Additions Module Makefile.
#
# (For 2.6.x this file must be 'Makefile'!)
#
# Copyright (C) 2006-2011 Oracle Corporation
#
# This file is part of VirtualBox Open Source Edition (OSE), as
# available from http://www.virtualbox.org. This file is free software;
# you can redistribute it and/or modify it under the terms of the GNU
# General Public License (GPL) as published by the Free Software
# Foundation, in version 2 as it comes in the "COPYING" file of the
# VirtualBox OSE distribution. VirtualBox OSE is distributed in the
# hope that it will be useful, but WITHOUT ANY WARRANTY of any kind.
#

# Linux kbuild sets this to our source directory if we are called from there

MOD_NAME = vboxsf
MOD_SRC    = $(srctree)/drivers/vbox/$(MOD_NAME)
MOD_OBJS   = \
	vfsmod.o \
	dirops.o \
	lnkops.o \
	regops.o \
	utils.o \
	GenericRequest.o \
	SysHlp.o \
	PhysHeap.o \
	Init.o \
	VMMDev.o \
	HGCM.o \
	VBoxGuestR0LibSharedFolders.o \
	VbglR0CanUsePhysPageList.o
ifeq ($(ARCH),x86)
MOD_OBJS  += \
	divdi3.o \
	moddi3.o \
	udivdi3.o \
	umoddi3.o \
	qdivrem.o
endif

MOD_INCL   = $(addprefix -I$(MOD_SRC),/ /include /r0drv/linux)
MANGLING := $(MOD_SRC)/include/VBox/VBoxGuestMangling.h


MOD_DEFS   = -DRT_OS_LINUX -DIN_RING0 -DIN_RT_R0 \
	    -DIN_SUP_R0 -DVBOX -DVBOX_WITH_HGCM -DIN_MODULE -DIN_GUEST_R0
# our module does not export any symbol
MOD_DEFS  += -DRT_NO_EXPORT_SYMBOL
ifeq ($(ARCH),amd64)
 MOD_DEFS += -DRT_ARCH_AMD64 -DVBOX_WITH_64_BITS_GUESTS
else
 MOD_DEFS += -DRT_ARCH_X86
endif

ifeq ($(KERN_VERSION), 24)
 MOD_CFLAGS =
else
 MOD_CFLAGS = -fshort-wchar -include $(MANGLING)

# special hack for Fedora Core 6 2.6.18 (fc6), rhel5 2.6.18 (el5),
# ClarkConnect 4.3 (cc4) and ClarkConnect 5 (v5)
 ifeq ($(KERNELRELEASE),)
  MOD_EXTRA += $(foreach inc,$(KERN_INCL),\
              $(if $(wildcard $(inc)/linux/utsrelease.h),\
                $(if $(shell grep '"2.6.18.*fc6.*"' $(inc)/linux/utsrelease.h; \
			    grep '"2.6.18.*el5.*"' $(inc)/linux/utsrelease.h; \
			    grep '"2.6.18.*v5.*"'  $(inc)/linux/utsrelease.h; \
			    grep '"2.6.18.*cc4.*"' $(inc)/linux/utsrelease.h),\
		-DKERNEL_FC6,),))
 else
  MOD_EXTRA += $(if $(shell echo "$(KERNELRELEASE)"|grep '2.6.18.*fc6.*';\
			echo "$(KERNELRELEASE)"|grep '2.6.18.*el5.*';\
			echo "$(KERNELRELEASE)"|grep '2.6.18.*v5.*';\
			echo "$(KERNELRELEASE)"|grep '2.6.18.*cc4.*'),\
		-DKERNEL_FC6,)
 endif
endif

 
obj-m		+= $(MOD_NAME).o
$(MOD_NAME)-objs := $(MOD_OBJS)
EXTRA_CFLAGS=$(MOD_INCL) $(MOD_DEFS)  $(MOD_CFLAGS)


