ARMGNU ?= aarch64-linux-gnu				# (?=) условное присвоение - оператор присваивает данное значение переменной,
							# если этой переменной уже не присвоенно какое либо значение.
COPS = -Wall -nostdlib -nostartfiles -ffreestanding -Iinclude -mgeneral-regs-only	# (=) при рекурсивном присвоение переменные 
ASMOPS = -Iinclude 					# не вычисяются в момент присвоения, но определяются в момент их выполнения
# Пример: GCC = gcc      
#         FLAGS = -W
# CC = ${GCC} {FLAGS}	will_converted_to=>	gcc -W		when       ${CC} file.c		executed
# if_changed	GCC = c++ 	then_next_time		${CC}   	will     	c++ -W
BUILD_DIR = build
SRC_DIR = src

all : kernel8.img	# all - цель компиляции которая зависит от других модулей

clean :			# удаление файлов появивщихся в результате шагов компиляции
	rm -rf $(BUILD_DIR) *.img 

$(BUILD_DIR)/%_c.o: $(SRC_DIR)/%.c # $(имя_пременной) использование ранее определенных переменных. % - групповой символ заменяющий 
	mkdir -p $(@D)		   # несколько символов.
	$(ARMGNU)-gcc $(COPS) -MMD -c $< -o $@  # $< - имя первого зависимого. $@ - полное имя текущей цели.

$(BUILD_DIR)/%_s.o: $(SRC_DIR)/%.S
	$(ARMGNU)-gcc $(ASMOPS) -MMD -c $< -o $@

C_FILES = $(wildcard $(SRC_DIR)/*.c)
ASM_FILES = $(wildcard $(SRC_DIR)/*.S)
OBJ_FILES = $(C_FILES:$(SRC_DIR)/%.c=$(BUILD_DIR)/%_c.o)
OBJ_FILES += $(ASM_FILES:$(SRC_DIR)/%.S=$(BUILD_DIR)/%_s.o)

DEP_FILES = $(OBJ_FILES:%.o=%.d)
-include $(DEP_FILES)

kernel8.img: $(SRC_DIR)/linker.ld $(OBJ_FILES)
	$(ARMGNU)-ld -T $(SRC_DIR)/linker.ld -o $(BUILD_DIR)/kernel8.elf  $(OBJ_FILES)
	$(ARMGNU)-objcopy $(BUILD_DIR)/kernel8.elf -O binary kernel8.img
