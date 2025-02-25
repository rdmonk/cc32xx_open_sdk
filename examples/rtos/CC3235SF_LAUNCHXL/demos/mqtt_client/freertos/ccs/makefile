SIMPLELINK_CC32XX_SDK_INSTALL_DIR ?= $(abspath ../../../../../../..)

include $(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/imports.mak

KERNEL_BUILD := $(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/kernel/freertos/builds/cc32xx/release

CC = "$(CCS_ARMCOMPILER)/bin/armcl"
LNK = "$(CCS_ARMCOMPILER)/bin/armcl"
HEX = "$(CCS_ARMCOMPILER)/bin/armobjcopy"

SYSCFG_CMD_STUB = $(SYSCONFIG_TOOL) --compiler ccs --product $(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/.metadata/product.json
SYSCFG_GEN_FILES := $(shell $(SYSCFG_CMD_STUB) --listGeneratedFiles --output . ../../freertos/mqtt_client.syscfg)
SYSCFG_REF_FILES := $(shell $(SYSCFG_CMD_STUB) --listReferencedFiles --output . ../../freertos/mqtt_client.syscfg)

ifeq ($(OS),Windows_NT)
	SYSCFG_FILES = $(subst \,/,$(SYSCFG_REF_FILES) $(SYSCFG_GEN_FILES))
else
	SYSCFG_FILES = $(SYSCFG_REF_FILES) $(SYSCFG_GEN_FILES)
endif
SYSCFG_C_FILES = $(filter %.c,$(SYSCFG_FILES))
SYSCFG_H_FILES = $(filter %.h,$(SYSCFG_FILES))

OBJECTS = mqtt_client_app.obj ifmod_mqtt_if.obj ifmod_wifi_if.obj ifmod_httpsrv_if.obj ifmod_utils_if.obj ifmod_uart_if.obj ifmod_ota_if.obj ifmod_ota_vendor_github.obj ifmod_ota_vendor_dropbox.obj freertos_main_freertos.obj $(patsubst %.c,%.obj,$(notdir $(SYSCFG_C_FILES)))
NAME = mqtt_client

CFLAGS = -I../.. \
    -I. \
      \
    -DDeviceFamily_CC3220 \
    "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source" \
    "-I$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/posix/ccs" \
    "-I$(FREERTOS_INSTALL_DIR)/FreeRTOS/Source/include" \
    "-I$(FREERTOS_INSTALL_DIR)/FreeRTOS/Source/portable/CCS/ARM_CM3" \
    "-I$(KERNEL_BUILD)" \
    --silicon_version=7M4 \
    --code_state=16 \
    --little_endian \
    --display_error_number \
    --diag_warning=255 \
    --diag_wrap=off \
    --gen_func_subsections=on \
    --float_support=vfplib \
    --symdebug:dwarf \
    "-I$(CCS_ARMCOMPILER)/include"

LFLAGS = "-i$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/net/mqtt/lib" \
    "-i$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/net/lib" \
    "-i$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/net/ota" \
    "-i$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/net/http/http_lib_for_OTA" \
    -lccs/m4/httpclient_release.a \
    "-i$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/utils/json/lib" \
    "-i$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/drivers/net/wifi/slnetif" \
    "-i$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source" \
    --diag_suppress=10063 \
    "-i$(SIMPLELINK_CC32XX_SDK_INSTALL_DIR)/source/ti/drivers/net/wifi" \
    ti_utils_build_linker.cmd.genlibs \
    "-l$(KERNEL_BUILD)/ccs/freertos.lib" \
    -lti/devices/cc32xx/driverlib/ccs/Release/driverlib.a \
    ../../freertos/ccs/cc32xxsf_freertos.cmd \
    "-m$(NAME).map" \
    --warn_sections \
    --display_error_number \
    --diag_wrap=off \
    --rom_model \
    "-i$(CCS_ARMCOMPILER)/lib" \
    -llibc.a
all: $(NAME).bin

$(KERNEL_BUILD)/ccs/freertos.lib:
	@ $(ECHOBLANKLINE)
	@ echo $@ is not built.
	@ echo You can build it by issuing $(MAKE) in $(dir $@).
	@ $(ECHOBLANKLINE)

.INTERMEDIATE: syscfg
$(SYSCFG_FILES): syscfg
	@ echo generation complete

syscfg: ../../freertos/mqtt_client.syscfg
	@ echo Generating configuration files...
	@ $(SYSCFG_CMD_STUB) --output $(@D) $<

define C_RULE
$(basename $(notdir $(1))).obj: $(1) $(SYSCFG_H_FILES)
	@ echo Building $$@
	@ $(CC) $(CFLAGS) $$< --output_file=$$@
endef

$(foreach c_file,$(SYSCFG_C_FILES),$(eval $(call C_RULE,$(c_file))))

mqtt_client_app.obj: ../../mqtt_client_app.c $(SYSCFG_H_FILES)
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

ifmod_mqtt_if.obj: ../../ifmod/mqtt_if.c $(SYSCFG_H_FILES)
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

ifmod_wifi_if.obj: ../../ifmod/wifi_if.c $(SYSCFG_H_FILES)
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

ifmod_httpsrv_if.obj: ../../ifmod/httpsrv_if.c $(SYSCFG_H_FILES)
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

ifmod_utils_if.obj: ../../ifmod/utils_if.c $(SYSCFG_H_FILES)
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

ifmod_uart_if.obj: ../../ifmod/uart_if.c $(SYSCFG_H_FILES)
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

ifmod_ota_if.obj: ../../ifmod/ota_if.c $(SYSCFG_H_FILES)
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

ifmod_ota_vendor_github.obj: ../../ifmod/ota_vendor_github.c $(SYSCFG_H_FILES)
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

ifmod_ota_vendor_dropbox.obj: ../../ifmod/ota_vendor_dropbox.c $(SYSCFG_H_FILES)
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

freertos_main_freertos.obj: ../../freertos/main_freertos.c $(SYSCFG_H_FILES)
	@ echo Building $@
	@ $(CC) $(CFLAGS) $< --output_file=$@

$(NAME).out: $(OBJECTS) $(KERNEL_BUILD)/ccs/freertos.lib
	@ echo linking $@
	@ $(LNK) -z $(OBJECTS)  $(LFLAGS) -o $(NAME).out

$(NAME).bin: $(NAME).out
	@ echo building $@
	@ $(HEX) -O binary --only-section .text --only-section .const --only-section .cinit --only-section .resetVecs $? $@

clean:
	@ echo Cleaning...
	@ $(RM) $(OBJECTS) > $(DEVNULL) 2>&1
	@ $(RM) $(NAME).out > $(DEVNULL) 2>&1
	@ $(RM) $(NAME).map > $(DEVNULL) 2>&1
	@ $(RM) $(NAME).bin > $(DEVNULL) 2>&1
	@ $(RM) $(SYSCFG_GEN_FILES)> $(DEVNULL) 2>&1
