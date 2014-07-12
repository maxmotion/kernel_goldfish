MOD_NAME   = vboxvideo
MOD_OBJS   = vboxvideo_drm.o
MOD_SRC    = $(srctree)/drivers/vbox/$(MOD_NAME)
MANGLING := $(MOD_SRC)/include/VBox/VBoxGuestMangling.h
MOD_CFLAGS = -fshort-wchar -include $(MANGLING)
MOD_INCL   = $(addprefix -I$(MOD_SRC),/ /include /r0drv/linux)
# Enterprise Linux 6.5 does not include the drm user API headers with the kernel
# headers.
MOD_INCL += $(foreach inc,$(KERN_INCL) include,\
              $(if $(wildcard $(inc)/linux/utsrelease.h),\
                $(if $(shell grep '"2.6.32.*el6.*"' $(inc)/linux/utsrelease.h),\
		      -I/usr/include,),))
MOD_DEFS  := -DRT_OS_LINUX -DIN_RING0 -DIN_RT_R0 \
	     -DIN_SUP_R0 -DVBOX -DVBOX_WITH_HGCM -DLOG_TO_BACKDOOR -DIN_MODULE \
	     -DIN_GUEST_R0
# our module does not export any symbol
MOD_DEFS  += -DRT_NO_EXPORT_SYMBOL
ifeq ($(ARCH),amd64)
 MOD_DEFS += -DRT_ARCH_AMD64 -DVBOX_WITH_64_BITS_GUESTS
else
 MOD_DEFS += -DRT_ARCH_X86
endif
obj-m		+= $(MOD_NAME).o
$(MOD_NAME)-objs := $(MOD_OBJS)
EXTRA_CFLAGS=$(MOD_INCL) $(MOD_DEFS)  $(MOD_CFLAGS)