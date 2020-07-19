#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# MakeFile for the C1M2 Week 2 Assignment
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      .i - Generates preprocessed targets of all c-program implementation files
#      .asm - Generates assembly output of c-program and final executable
#      .o - generates object file for all c-source files 
#      compile-all - compile all object files and do not link
#      Build - compile all object files and link into one .exe
#
# Platform Overrides:
#      Performs overide for either compiling and running on HOST or MSP432 platform
#
#------------------------------------------------------------------------------
include sources.mk


# Platform Overrides
PLATFORM = HOST

#General Flags
GEN = -Wall -Werror -g -O0 

# Architectures Specific Flags
LINKER_FILE = -T msp432p401r.lds
CPU = cortex-m4
ARCH = armv7e-m
SPECS = nosys.specs

PLATFORM_FLAGS = -m$(ARCH) - mcpu=$(CPU) -specs=$(SPECS)
OBJS = $(SOURCES:.c=.o)

# Compiler Flags and Defines
TARGET = c1m2
CSTD = c99
LD = arm-none-eabi-ld 
LDFLAGS = -Wl, Map=$(TARGET).map -T$(LINKER_FILE)

CFLAGS = -g -std=$(CSTD) # -mcpu=$(CPU) 
CPPFLAGs = -Wall -Werror -g -O0 -std=$(CSTD) $(INCLUDES)


ifeq ($(PLATFORM), HOST)
	CC = gcc
	CFLAGS = -DHOST
	LD = ld
endif

ifeq ($(PLATFORM), MSP432)
	CC = arm-none-eabi-gcc
	CFLAGS = -DMSP432 -mcpu=$(CPU) -march=$(ARCH) --specs = $(SPEC) -mfloat-abi=hard -mfpu=fpv4-sp-d16
	LD = arm-non-eabi-ld
endif

# Build Targets

%.o: %.c
	$(CC) -c $< $(CFLAGS) $(CPPFLAGS) -o $@

%.i: %.c 
	$(CC) -E $< $(CFLAGS) $(CPPFLAGS) -o $@

%.asm: %.c
	$(CC) -c $< $(CFLAGS) $(CPPFLAGS) -o $@
	objdump -S $@

%.d : %.c
	$(CC) -M $< $(CFLAGS) $(CPPFLAGS) -o $@


#Phony Protection

.PHONY: compile
compile: all

.PHONY: build
build: all

.PHONY: all 
all: $(TARGET).out

$(TARGET).out: $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(CPPFLAGS) $(LDFLAGS) -o $@


.PHONY: clean
	rm -f $(OBJS) $(TARGET).out $(TARGET).map *asm *i

