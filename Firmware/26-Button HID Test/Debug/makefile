################################################################################
# Automatically-generated file. Do not edit!
################################################################################

-include ../makefile.init

RM := rm -rf

# All of the sources participating in the build are defined here
-include sources.mk
-include Startup/subdir.mk
-include Src/subdir.mk
-include Middlewares/ST/STM32_USB_Device_Library/Core/Src/subdir.mk
-include Middlewares/ST/STM32_USB_Device_Library/Class/CustomHID/Src/subdir.mk
-include Drivers/STM32F1xx_HAL_Driver/Src/subdir.mk
-include subdir.mk
-include objects.mk

ifneq ($(MAKECMDGOALS),clean)
ifneq ($(strip $(C_DEPS)),)
-include $(C_DEPS)
endif
endif

-include ../makefile.defs

# Add inputs and outputs from these tool invocations to the build variables 
EXECUTABLES += \
26-Button\ HID\ Test.elf \

SIZE_OUTPUT += \
default.size.stdout \

OBJDUMP_LIST += \
26-Button\ HID\ Test.list \

OBJCOPY_HEX += \
26-Button\ HID\ Test.hex \


# All Target
all: 26-Button\ HID\ Test.elf secondary-outputs

# Tool invocations
26-Button\ HID\ Test.elf: $(OBJS) $(USER_OBJS) L:\Documents\GitHub\GameVision-50-Re-Engineering\Firmware\26-Button\ HID\ Test\STM32F103C8TX_FLASH.ld
	arm-none-eabi-gcc -o "26-Button HID Test.elf" @"objects.list" $(USER_OBJS) $(LIBS) -mcpu=cortex-m3 -T"L:\Documents\GitHub\GameVision-50-Re-Engineering\Firmware\26-Button HID Test\STM32F103C8TX_FLASH.ld" --specs=nosys.specs -Wl,-Map="26-Button HID Test.map" -Wl,--gc-sections -static --specs=nano.specs -mfloat-abi=soft -mthumb -Wl,--start-group -lc -lm -Wl,--end-group
	@echo 'Finished building target: $@'
	@echo ' '

default.size.stdout: $(EXECUTABLES)
	arm-none-eabi-size  $(EXECUTABLES)
	@echo 'Finished building: $@'
	@echo ' '

26-Button\ HID\ Test.list: $(EXECUTABLES)
	arm-none-eabi-objdump -h -S $(EXECUTABLES) > "26-Button HID Test.list"
	@echo 'Finished building: $@'
	@echo ' '

26-Button\ HID\ Test.hex: $(EXECUTABLES)
	arm-none-eabi-objcopy  -O ihex $(EXECUTABLES) "26-Button HID Test.hex"
	@echo 'Finished building: $@'
	@echo ' '

# Other Targets
clean:
	-$(RM) *
	-@echo ' '

secondary-outputs: $(SIZE_OUTPUT) $(OBJDUMP_LIST) $(OBJCOPY_HEX)

.PHONY: all clean dependents
.SECONDARY:

-include ../makefile.targets
