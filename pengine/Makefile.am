#
# Copyright (C) 2004 Andrew Beekhof
#
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License
# as published by the Free Software Foundation; either version 2
# of the License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA  02111-1307, USA.
#
include $(top_srcdir)/Makefile.common

AM_CPPFLAGS     += -I$(top_builddir) -I$(top_srcdir)

halibdir	= $(CRM_DAEMON_DIR)

PE_TESTS	= $(wildcard test10/*.scores)

testdir			= $(datadir)/$(PACKAGE)/tests/pengine
test_SCRIPTS		= regression.sh
test_DATA		= regression.core.sh

test10dir		= $(datadir)/$(PACKAGE)/tests/pengine/test10
test10_DATA		= $(PE_TESTS) $(PE_TESTS:%.scores=%.xml) $(PE_TESTS:%.scores=%.exp) $(PE_TESTS:%.scores=%.dot) $(PE_TESTS:%.scores=%.summary) $(wildcard test10/*.stderr)

COMMONLIBS	= 					\
		$(top_builddir)/lib/common/libcrmcommon.la		\
		$(top_builddir)/lib/pengine/libpe_status.la		\
		libpengine.la $(CURSESLIBS) $(CLUSTERLIBS)

beekhof:
	echo $(shell ls -1 test10/*.xml)

#TESTS = test10/*.xml
TESTS = test10/bug-rh-1097457.xml
TEST_EXTENSIONS = .xml
XML_LOG_COMPILER = ./regression.sh
AM_XML_LOG_FLAGS = -V --run 
#LOG_COMPILER = 
#AM_LOG_FLAGS = -V


## libraries
lib_LTLIBRARIES	= libpengine.la

## binary progs
halib_PROGRAMS	= pengine

if BUILD_XML_HELP
man7_MANS =	pengine.7
endif

## SOURCES
noinst_HEADERS	= allocate.h utils.h pengine.h
#utils.h pengine.h

libpengine_la_LDFLAGS	= -version-info 8:0:0
# -L$(top_builddir)/lib/pils -lpils -export-dynamic -module -avoid-version
libpengine_la_SOURCES	= pengine.c allocate.c utils.c constraints.c
libpengine_la_SOURCES  += native.c group.c clone.c master.c graph.c

libpengine_la_LIBADD    = $(top_builddir)/lib/pengine/libpe_status.la \
	$(top_builddir)/lib/cib/libcib.la

pengine_SOURCES	= main.c
pengine_LDADD	= $(top_builddir)/lib/cib/libcib.la $(COMMONLIBS)
# libcib for get_object_root()
#		$(top_builddir)/lib/hbclient/libhbclient.la

install-exec-local:
	$(mkinstalldirs) $(DESTDIR)/$(PE_STATE_DIR)
	-chown $(CRM_DAEMON_USER) $(DESTDIR)/$(PE_STATE_DIR)
	-chgrp $(CRM_DAEMON_GROUP) $(DESTDIR)/$(PE_STATE_DIR)
	-chmod 750 $(DESTDIR)/$(PE_STATE_DIR)

uninstall-local:

clean-generic:
	rm -f test10/*.pe.*
