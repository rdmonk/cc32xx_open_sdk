
# Find subdirectories
RELEASE-CCS-SUBDIRS     = $(wildcard ./ccs/Release/.)
RELEASE-TICLANG-SUBDIRS = $(wildcard ./ticlang/Release/.)
RELEASE-GCC-SUBDIRS     = $(wildcard ./gcc/Release/.)
RELEASE-IAR-SUBDIRS     = $(wildcard ./iar/Release/.)

include ../../../../imports.mak

RELEASE-SUBDIRS =
ifneq ($(CCS_ARMCOMPILER),)
	RELEASE-SUBDIRS += $(RELEASE-CCS-SUBDIRS)
endif
ifneq ($(TICLANG_ARMCOMPILER),)
	RELEASE-SUBDIRS += $(RELEASE-TICLANG-SUBDIRS)
endif
ifneq ($(GCC_ARMCOMPILER),)
	RELEASE-SUBDIRS += $(RELEASE-GCC-SUBDIRS)
endif
ifneq ($(IAR_ARMCOMPILER),)
	RELEASE-SUBDIRS += $(RELEASE-IAR-SUBDIRS)
endif

.PHONY: release $(RELEASE-SUBDIRS)

# rules for building
all: release

release: $(RELEASE-SUBDIRS)

$(RELEASE-SUBDIRS):
	@ $(MAKE) -C $@ -f makefile.defs

CLEAN-RELEASE-SUBDIRS = $(addprefix clean-,$(RELEASE-SUBDIRS))

.PHONY: clean-release $(CLEAN-RELEASE-SUBDIRS)

# rules for cleaning
clean: clean-release

clean-release: $(CLEAN-RELEASE-SUBDIRS)

$(CLEAN-RELEASE-SUBDIRS):
	@ $(MAKE) -C $(subst clean-,,$@) -f makefile.defs clean
