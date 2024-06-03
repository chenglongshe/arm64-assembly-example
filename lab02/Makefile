ARMGNU ?= aarch64-linux-gnu

COPS += -DCONFIG_BOARD_PI4B
QEMU_FLAGS  += -machine raspi4b

COPS += -g -Wall -nostdlib -nostdinc -Iinclude
ASMOPS = -g -Iinclude 

BUILD_DIR = build
SRC_DIR = src

all : benos.bin

clean :
	rm -rf $(BUILD_DIR) *.bin 

$(BUILD_DIR)/%_c.o: $(SRC_DIR)/%.c
	mkdir -p $(@D)
	$(ARMGNU)-gcc $(COPS) -MMD -c $< -o $@

$(BUILD_DIR)/%_s.o: $(SRC_DIR)/%.S
	$(ARMGNU)-gcc $(ASMOPS) -MMD -c $< -o $@

C_FILES = $(wildcard $(SRC_DIR)/*.c)
ASM_FILES = $(wildcard $(SRC_DIR)/*.S)
OBJ_FILES = $(C_FILES:$(SRC_DIR)/%.c=$(BUILD_DIR)/%_c.o)
OBJ_FILES += $(ASM_FILES:$(SRC_DIR)/%.S=$(BUILD_DIR)/%_s.o)

DEP_FILES = $(OBJ_FILES:%.o=%.d)
-include $(DEP_FILES)

benos.bin: $(SRC_DIR)/linker.ld $(OBJ_FILES)
	$(ARMGNU)-ld -T $(SRC_DIR)/linker.ld -o $(BUILD_DIR)/nogos.elf  $(OBJ_FILES)
	$(ARMGNU)-objcopy $(BUILD_DIR)/nogos.elf -O binary nogos.bin

QEMU_FLAGS  += -nographic

run:
	qemu-system-aarch64 $(QEMU_FLAGS) -kernel nogos.bin
debug:
	qemu-system-aarch64 $(QEMU_FLAGS) -kernel nogos.bin -S -s
