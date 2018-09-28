# Makefile for cgi-src
#
# Copyright © 1995 by Jef Poskanzer <jef@mail.acme.com>.
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
# 1. Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
# 2. Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in the
#    documentation and/or other materials provided with the distribution.
# 
# THIS SOFTWARE IS PROVIDED BY THE AUTHOR AND CONTRIBUTORS ``AS IS'' AND
# ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
# ARE DISCLAIMED.  IN NO EVENT SHALL THE AUTHOR OR CONTRIBUTORS BE LIABLE
# FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
# DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS
# OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
# HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
# LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY
# OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF
# SUCH DAMAGE.

prefix =	/usr
exec_prefix =	/usr
WEBDIR =	$(prefix)/www
CGIBINDIR =	$(WEBDIR)/cgi-bin
MANDIR =	/usr/share/man

CC =		arm-ubinux-linux-gnueabi-gcc  -march=armv7-a -marm  -mthumb-interwork -mfloat-abi=hard -mfpu=neon -mtune=cortex-a9 --sysroot=/ubinux/ubinux16/build-armv7-lighttpd/tmp/sysroots/ubinux-armv7
CCOPT =		-O2
DEFS =		-DPACKAGE_NAME=\"\" -DPACKAGE_TARNAME=\"\" -DPACKAGE_VERSION=\"\" -DPACKAGE_STRING=\"\" -DPACKAGE_BUGREPORT=\"\" -DPACKAGE_URL=\"\" -DHAVE__PROGNAME=1 -DSTDC_HEADERS=1 -DHAVE_SYS_TYPES_H=1 -DHAVE_SYS_STAT_H=1 -DHAVE_STDLIB_H=1 -DHAVE_STRING_H=1 -DHAVE_MEMORY_H=1 -DHAVE_STRINGS_H=1 -DHAVE_INTTYPES_H=1 -DHAVE_STDINT_H=1 -DHAVE_UNISTD_H=1 -DHAVE_FCNTL_H=1 -DHAVE_GRP_H=1 -DHAVE_MEMORY_H=1 -DHAVE_PATHS_H=1 -DHAVE_POLL_H=1 -DHAVE_SYS_POLL_H=1 -DTIME_WITH_SYS_TIME=1 -DHAVE_DIRENT_H=1 -DHAVE_LIBCRYPT=1 -DHAVE_STRERROR=1 -DHAVE_WAITPID=1 -DHAVE_VSNPRINTF=1 -DHAVE_DAEMON=1 -DHAVE_SETSID=1 -DHAVE_GETADDRINFO=1 -DHAVE_GETNAMEINFO=1 -DHAVE_GAI_STRERROR=1 -DHAVE_SIGSET=1 -DHAVE_ATOLL=1 -DHAVE_STDLIB_H=1 -DHAVE_UNISTD_H=1 -DHAVE_SYS_PARAM_H=1 -DHAVE_GETPAGESIZE=1 -DHAVE_MMAP=1 -DHAVE_SELECT=1 -DHAVE_POLL=1 -DHAVE_TM_GMTOFF=1 -DHAVE_INT64T=1 -DHAVE_SOCKLENT=1
INCLS =		-I..
CFLAGS =	$(CCOPT) $(DEFS) $(INCLS)
LDFLAGS =	-Wl,-O1 -Wl,--hash-style=gnu -Wl,--as-needed -Wl,-z,relro,-z,now 
LIBS =		-lcrypt 
NETLIBS =	
INSTALL =	/usr/bin/install -c

CLEANFILES =	*.o redirect ssi phf



.c.o:
	@rm -f $@
	$(CC) $(CFLAGS) -c $*.c

all:		redirect ssi phf

redirect:	redirect.o
	$(CC) $(LDFLAGS) $(STATICFLAG) redirect.o $(LIBS) -o redirect

ssi:		ssi.o ../match.o
	$(CC) $(LDFLAGS) $(STATICFLAG) ssi.o ../match.o $(LIBS) -o ssi

ssi.o:		../match.h

phf:		phf.o
	$(CC) $(LDFLAGS) $(STATICFLAG) phf.o $(LIBS) -o phf

strerror.o:
	@rm -f strerror.o
	@ln -s ../strerror.o
	cd .. ; $(MAKE) $(MFLAGS) strerror.o

install:	all
	install -d $(DESTDIR)$(CGIBINDIR)
	install -m 0755 redirect $(DESTDIR)$(CGIBINDIR)/redirect
	install -d $(DESTDIR)$(MANDIR)/man8
	install -m 0644 redirect.8 $(DESTDIR)$(MANDIR)/man8/redirect.8
	install -m 0755 ssi $(DESTDIR)$(CGIBINDIR)/ssi
	install -m 0644 ssi.8 $(DESTDIR)$(MANDIR)/man8/ssi.8
	install -m 0755 phf $(DESTDIR)$(CGIBINDIR)/phf

clean:
	rm -f $(CLEANFILES)

distclean:
	rm -f $(CLEANFILES) Makefile
