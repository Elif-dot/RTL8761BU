FW_DIR	:= /lib/firmware
FW_NAME := rtl8761bu_fw
CFG_NAME := rtl8761bu_config

all:
	@echo "Copy 8761BUV firmware to $(FW_DIR)/$(FW_NAME)"
	@cp -f $(FW_NAME) $(FW_DIR)/$(FW_NAME)
	@echo "Copy 8761BUV Config to $(FW_DIR)/$(CFG_NAME)"
	@cp -f $(CFG_NAME) $(FW_DIR)/$(CFG_NAME)

clean:
	rm -f $(FW_DIR)/${FW_NAME}
	rm -f $(FW_DIR)/${CFG_NAME}

