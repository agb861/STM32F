
# Put your stlink folder here so make burn will work.
STLINK=~/stlink.git

# Put your source files here (or *.c, etc)
SRCS=main.c system_stm32f2xx.c  extra_iar.c
SRCS +=  newlib_stubs.c
SRCS += stm32f2xx_gpio.c stm32f2xx_rcc.c stm32f2xx_usart.c stm32f2xx_flash.c stm32f2xx_dma.c stm32f2xx_adc.c stm32f2xx_spi.c
SRCS += stm32f2xx_tim.c stm32f2xx_exti.c stm32f2xx_i2c.c stm32f2xx_rng.c stm32f2xx_rtc.c stm32f2xx_sdio.c  stm32f2xx_syscfg.c 
SRCS += stm32f2xx_pwr.c misc.c
# Binaries will be generated with this name (.elf, .bin, .hex, etc)
PROJ_NAME=uart

# Put your STM32F2 library code directory here
STM_COMMON=./bsp/STM32F2xx_StdPeriph_Lib_V1.1.0/

# Normally you shouldn't need to change anything below this line!
#######################################################################################

ARM=/home/wouters/GIT/willemwouters/STM32_EMW3162/bsp/gcc-arm-none-eabi-4_9-2014q4-20141203/install-native/
PREFIX=$(ARM)bin/
CC=$(PREFIX)arm-none-eabi-gcc
LD=$(PREFIX)arm-none-eabi-ld

OBJCOPY=arm-none-eabi-objcopy

CFLAGS  = -g -O0 -Wall -Tstm32_flash.ld  
CFLAGS += -mlittle-endian -mthumb -mcpu=cortex-m3
CFLAGS += -msoft-float -DSTM32F2 -fshort-wchar
CFLAGS += -I. -I ./lib -L ./lib -DUSE_STDPERIPH_DRIVER

# Include files from STM libraries
CFLAGS += -I$(STM_COMMON)/Utilities/STM32F2-Discovery
CFLAGS += -I$(STM_COMMON)/Libraries/CMSIS/Include -I$(STM_COMMON)/Libraries/CMSIS/Device/ST/STM32F2xx/Include
CFLAGS += -I$(STM_COMMON)/Libraries/STM32F2xx_StdPeriph_Driver/inc

#OBJS = $(SRCS:.c=.o)
OBJS =  $(patsubst %.c, %.o, $(SRCS))
OBJSASM =  $(patsubst %.s, %.o, $(SRCS))

#OBJS +=  $(patsubst %.s, %.o, $(SRCS))
#OBJS += $(SRCS:.s=.o)


LDLIBS = -L$(ARM)arm-none-eabi/lib/ -L$(ARM)lib/gcc/arm-none-eabi/4.9.3/ 
.PHONY: proj

all: proj

proj: $(PROJ_NAME).elf

$(PROJ_NAME).elf: ./startup_stm32f2xx.o $(OBJS)
	$(LD) -nostartfiles  -o $@ $(OBJS) startup_stm32f2xx.o $(LDLIBS) ./lib/mxchipWNet_3162.lib -Tstm32_flash.ld 
	$(OBJCOPY) -O ihex $(PROJ_NAME).elf $(PROJ_NAME).hex
	$(OBJCOPY) -O binary $(PROJ_NAME).elf $(PROJ_NAME).bin

%.o: %.c
	$(CC) -c $(CFLAGS) $< -o $@
	
%.o: %.s
	$(CC) -c $(CFLAGS) $< -o $@

clean:
	rm -f *.o $(PROJ_NAME).elf $(PROJ_NAME).hex $(PROJ_NAME).bin

# Flash the STM32F4
burn: proj
	$(STLINK)/st-flash write $(PROJ_NAME).bin 0x8000000
