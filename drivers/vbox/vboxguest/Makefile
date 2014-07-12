# $Revision: 83575 $
## @file
# VirtualBox Guest Additions Module Makefile.
#

#
# Copyright (C) 2006-2012 Oracle Corporation
#
# This file is part of VirtualBox Open Source Edition (OSE), as
# available from http://www.virtualbox.org. This file is free software;
# you can redistribute it and/or modify it under the terms of the GNU
# General Public License (GPL) as published by the Free Software
# Foundation, in version 2 as it comes in the "COPYING" file of the
# VirtualBox OSE distribution. VirtualBox OSE is distributed in the
# hope that it will be useful, but WITHOUT ANY WARRANTY of any kind.
#



MOD_NAME = vboxguest
MOD_SRC    = $(srctree)/drivers/vbox/$(MOD_NAME)
MOD_OBJS   = \
	VBoxGuest-linux.o \
	VBoxGuest.o \
	VBoxGuest2.o \
	GenericRequest.o \
	HGCMInternal.o \
	Init.o \
	PhysHeap.o \
	SysHlp.o \
	VMMDev.o \
	r0drv/alloc-r0drv.o \
	r0drv/initterm-r0drv.o \
	r0drv/memobj-r0drv.o \
	r0drv/mpnotification-r0drv.o \
	r0drv/powernotification-r0drv.o \
	r0drv/linux/alloc-r0drv-linux.o \
	r0drv/linux/assert-r0drv-linux.o \
	r0drv/linux/initterm-r0drv-linux.o \
	r0drv/linux/memobj-r0drv-linux.o \
	r0drv/linux/memuserkernel-r0drv-linux.o \
	r0drv/linux/mp-r0drv-linux.o \
	r0drv/linux/mpnotification-r0drv-linux.o \
	r0drv/linux/process-r0drv-linux.o \
	r0drv/linux/semevent-r0drv-linux.o \
	r0drv/linux/semeventmulti-r0drv-linux.o \
	r0drv/linux/semfastmutex-r0drv-linux.o \
	r0drv/linux/semmutex-r0drv-linux.o \
	r0drv/linux/spinlock-r0drv-linux.o \
	r0drv/linux/thread-r0drv-linux.o \
	r0drv/linux/thread2-r0drv-linux.o \
	r0drv/linux/time-r0drv-linux.o \
	r0drv/linux/RTLogWriteDebugger-r0drv-linux.o \
	r0drv/generic/semspinmutex-r0drv-generic.o \
	common/alloc/alloc.o \
	common/err/RTErrConvertFromErrno.o \
	common/err/RTErrConvertToErrno.o \
	common/log/log.o \
        common/log/logellipsis.o \
        common/log/logrel.o \
        common/log/logrelellipsis.o \
        common/log/logcom.o \
	common/log/logformat.o \
	common/misc/RTAssertMsg1Weak.o \
	common/misc/RTAssertMsg2.o \
	common/misc/RTAssertMsg2Add.o \
	common/misc/RTAssertMsg2AddWeak.o \
	common/misc/RTAssertMsg2AddWeakV.o \
	common/misc/RTAssertMsg2Weak.o \
	common/misc/RTAssertMsg2WeakV.o \
	common/misc/assert.o \
	common/misc/thread.o \
	common/string/RTStrCopy.o \
	common/string/RTStrCopyP.o \
	common/string/strformat.o \
	common/string/strformatrt.o \
	common/string/strformattype.o \
	common/string/strprintf.o \
	common/string/strtonum.o \
	common/table/avlpv.o \
	common/time/time.o \
	generic/RTAssertShouldPanic-generic.o \
        generic/RTLogWriteStdErr-stub-generic.o \
        generic/RTLogWriteStdOut-stub-generic.o \
	generic/RTSemEventWait-2-ex-generic.o \
	generic/RTSemEventWaitNoResume-2-ex-generic.o \
	generic/RTSemEventMultiWait-2-ex-generic.o \
	generic/RTSemEventMultiWaitNoResume-2-ex-generic.o \
	generic/errvars-generic.o \
	VBox/log-vbox.o \
	VBox/logbackdoor.o
ifeq ($(ARCH),x86)
MOD_OBJS += \
	common/math/gcc/divdi3.o \
	common/math/gcc/moddi3.o \
	common/math/gcc/udivdi3.o \
	common/math/gcc/umoddi3.o \
	common/math/gcc/qdivrem.o
endif
ifeq ($(ARCH),amd64)
MOD_OBJS +=	common/alloc/heapsimple.o
endif

MOD_DEFS = -DVBOX -DRT_OS_LINUX -DIN_RING0 -DIN_RT_R0 -DIN_GUEST \
            -DIN_GUEST_R0 -DIN_MODULE -DRT_WITH_VBOX -DVBGL_VBOXGUEST \
            -DVBOX_WITH_HGCM
ifeq ($(ARCH),amd64)
 MOD_DEFS  += -DRT_ARCH_AMD64
else
 MOD_DEFS  += -DRT_ARCH_X86
endif
ifeq ($(ARCH),amd64)
 MOD_DEFS += -DVBOX_WITH_64_BITS_GUESTS
endif

MOD_INCL   = $(addprefix -I$(MOD_SRC),/ /include /r0drv/linux)
MANGLING := $(MOD_SRC)/include/VBox/VBoxGuestMangling.h

ifeq ($(KERN_VERSION),24)
 ## @todo move to MOD_DEFS when we have finished refactoring
 MOD_CFLAGS = -DEXPORT_SYMTAB
else
 MOD_CFLAGS = -include $(MANGLING)
endif


obj-m		+= $(MOD_NAME).o
$(MOD_NAME)-objs := $(MOD_OBJS)
EXTRA_CFLAGS=$(MOD_INCL) $(MOD_DEFS) $(MOD_CFLAGS)

