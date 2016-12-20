TARGET?=i386-unknown-chronos-gnu
CRYSTAL_FLAGS?=--emit obj --cross-compile --target "$(TARGET)" --prelude=empty
LDFLAGS?=-m elf_i386
NASMFLAGS?=

DESTDIR?=
PREFIX?=/usr/local
EXEC_PREFIX=$(PREFIX)
BOOTDIR?=$(EXEC_PREFIX)/boot

ARCHDIR:=arch/$(HOSTARCH)

include $(ARCHDIR)/make.conf

OBJS:=\
$(ARCH_OBJS) \
src/kernel.o

OBJ_LINK:=\
$(OBJS)

all: nuummite.kern

.PHONY: all nuummite.kern install install-kern

nuummite.kern: $(OBJ_LINK) $(ARCHDIR)/linker.ld
	$(LD) -T $(ARCHDIR)/linker.ld -o $@ $(OBJ_LINK) $(LDFLAGS)

%.o: %.cr
	crystal build $(CRYSTAL_FLAGS) $< -o $(shell echo $@ | cut -f 1 -d '.')

%.o: %.asm
	$(NASM) $(NASMFLAGS) $< -o $@

install: install-kern

install-kern: nuummite.kern
	mkdir -p $(DESTDIR)$(BOOTDIR)
	cp nuummite.kern $(DESTDIR)$(BOOTDIR)