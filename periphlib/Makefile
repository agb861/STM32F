# Tools
PREFIX=arm-none-eabi-
CC=$(PREFIX)gcc
CCLD=$(PREFIX)gcc
LD=$(PREFIX)ld
AR=$(PREFIX)ar
AS=$(PREFIX)as
OBJCOPY=$(PREFIX)objcopy

VERBOSE=1

ifeq ($(VERBOSE), 1)
	V=
else
	V=@
endif
	
//FILENAME:=  $(patsubst %.c,%.o,$(wildcard *.c))

BINARY = libplaform.a
SOURCEDIR = src

INCLUDEFLAGS = -I .  -I ./inc -I ./src/CMSIS 
CCFLAGS = -mcpu=cortex-m3 -mthumb -DSTM32
BUILDDIR = objs

CFLAGS = $(INCLUDEFLAGS) $(CCFLAGS)
SOURCES := $(shell find  $(SOURCEDIR) -name '*.c')
OBJECTS := $(addprefix $(BUILDDIR)/,$(SOURCES:%.c=%.o))

all: $(BINARY)

clean:
	$(V)rm -rf ./objs
	$(V)rm $(BINARY)

$(BINARY): $(OBJECTS)
	$(V)$(AR) rcs $(BINARY) $(OBJECTS)

$(BUILDDIR)/%.o : %.c
	@mkdir -p $(@D)
	$(V)$(CC) $(CFLAGS) -c $< -o $@

