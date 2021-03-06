################################################################################
##
## Filename:	Makefile
##
## Project:	OpenArty, an entirely open SoC based upon the Arty platform
##
## Purpose:	
##
## Creator:	Dan Gisselquist, Ph.D.
##		Gisselquist Technology, LLC
##
################################################################################
##
## Copyright (C) 2017, Gisselquist Technology, LLC
##
## This program is free software (firmware): you can redistribute it and/or
## modify it under the terms of  the GNU General Public License as published
## by the Free Software Foundation, either version 3 of the License, or (at
## your option) any later version.
##
## This program is distributed in the hope that it will be useful, but WITHOUT
## ANY WARRANTY; without even the implied warranty of MERCHANTIBILITY or
## FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
## for more details.
##
## You should have received a copy of the GNU General Public License along
## with this program.  (It's in the $(ROOT)/doc directory, run make with no
## target there if the PDF file isn't present.)  If not, see
## <http://www.gnu.org/licenses/> for a copy.
##
## License:	GPL, v3, as defined and found on www.gnu.org,
##		http://www.gnu.org/licenses/gpl.html
##
##
################################################################################
##
##
AS := zip-as
CC := zip-gcc
AR := zip-ar
OBJDIR  := obj-zip
CFLAGS  := -O3
LIBSRCS := udiv.c umod.c s6board.c crt0.c kpanic.s
LIBOBJS := $(addprefix $(OBJDIR)/,$(subst .s,.o,$(subst .c,.o,$(LIBSRCS))))
all: libs6.a

$(OBJDIR)/%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

$(OBJDIR)/%.s: %.c
	$(CC) $(CFLAGS) -fdump-rtl-all -S $< -o $@

$(OBJDIR)/%.o: %.s
	$(AS) $< -o $@

$(OBJDIR)/crt0.o: crt0.c
	$(CC) $(CFLAGS) -ffreestanding -c $< -o $@

libs6.a: $(LIBOBJS)
	$(AR) -cru $@ $(LIBOBJS)

clean:
	rm -rf libs6.a $(OBJDIR)/

define	build-depends
	@echo "Building dependency file(s)"
	@$(CC) $(CFLAGS) -MM $(LIBSRCS) > $(OBJDIR)/xdepends.txt
	@sed -e 's/^.*.o: /$(OBJDIR)\/&/' < $(OBJDIR)/xdepends.txt > $(OBJDIR)/depends.txt
	@rm $(OBJDIR)/xdepends.txt
endef

tags: $(LIBSRCS) $(HEADERS)
	@echo "Generating tags"
	@ctags $(LIBSRCS) $(HEADERS)

$(OBJDIR)/depends.txt: $(OBJDIR)/
	$(build-depends)

.PHONY: depends
depends:
	$(build-depends)

$(OBJDIR)/:
	@bash -c "if [ ! -e $(OBJDIR) ]; then mkdir -p $(OBJDIR)/; fi"

-include $(OBJDIR)/depends.txt
