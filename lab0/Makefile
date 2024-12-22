ARMGNU ?= aarch64-linux-gnu
ARMQEMU ?= qemu-system-aarch64
RM ?= rm -f
K8 = kernel8
LINKER = linker.ld

QEMU_OPTS = -M raspi3b -kernel $(K8).img -display none -d in_asm

SRC_S = $(wildcard *.S)

OBJS = $(SRC_S:%.S=%.o)

.PHONY: all qemu clean

all: $(K8).img

%.o: %.S
	$(ARMGNU)-gcc -o $@ -c $<

$(K8).img: $(OBJS)
	$(ARMGNU)-ld -T $(LINKER) -o $(K8).elf $^
	$(ARMGNU)-objcopy -O binary $(K8).elf $@

qemu: $(K8).img
	$(ARMQEMU) $(QEMU_OPTS)

clean:
	$(RM) $(OBJS) $(K8).elf $(K8).img
