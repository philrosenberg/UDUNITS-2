# Copyright 2013 University Corporation for Atmospheric Research
#
# This file is part of the UDUNITS-2 package.  See the file COPYRIGHT
# in the top-level source-directory of the package for copying and
# redistribution conditions.
#
## Process this file with automake to produce Makefile.in
SUBDIRS	= xmlFailures xmlSuccesses
lib_LTLIBRARIES = libudunits2.la
libudunits2_la_SOURCES = unitcore.c converter.c formatter.c \
    idToUnitMap.c idToUnitMap.h \
    unitToIdMap.c unitToIdMap.h \
    unitAndId.c unitAndId.h \
    systemMap.c systemMap.h \
    prefix.c prefix.h \
    parser.y \
    status.c \
    xml.c \
    error.c \
    ut_free_system.c
BUILT_SOURCES = parser.c scanner.c
pkgdata_DATA = \
    udunits2.xml \
    udunits2-accepted.xml \
    udunits2-base.xml \
    udunits2-common.xml \
    udunits2-derived.xml \
    udunits2-prefixes.xml
TEXINFO_TEX	= ../texinfo.tex
info_TEXINFOS 	= udunits2lib.texi
#html_DATA = udunits2lib.html
#pdf_DATA = udunits2lib.pdf
AM_MAKEINFOFLAGS = -I $(top_srcdir)
AM_MAKEINFOHTMLFLAGS = --no-split -I $(top_srcdir)
libudunits2_la_LDFLAGS	= -version-number 0:1:0

lex_prefix = ut
LEX_OUTPUT_ROOT = lex.$(lex_prefix)
LFLAGS = -d -P$(lex_prefix)

include_HEADERS = udunits2.h converter.h
EXTRA_DIST = \
    CMakeLists.txt \
    parser.c \
    scanner.l \
    scanner.c \
    udunits2.xml \
    udunits2-accepted.xml \
    udunits2-base.xml \
    udunits2-common.xml \
    udunits2-derived.xml \
    udunits2-prefixes.xml \
    udunits2lib.html \
    udunits2lib.pdf
AM_YFLAGS = -t -p ut

if HAVE_CUNIT
LDADD			= \
    libudunits2.la \
    @LD_CUNIT@ \
    @LIBS_COVERAGE@ \
    @LIBS@
check_PROGRAMS		= testUnits
TESTS_ENVIRONMENT	= UDUNITS2_XML_PATH='$(srcdir)/udunits2.xml'
TESTS = testUnits
else
LDADD			= @LIBS@
endif

if ENABLE_UDUNITS_1
libudunits2_la_SOURCES += udunits-1.c
include_HEADERS += udunits.h
if HAVE_CUNIT
check_PROGRAMS += testUnits-1
TESTS += testUnits-1
endif
endif

DISTCLEANFILES	= *.log
MOSTLYCLEANFILES = lint.log *.ln *.i core core.[0-9]* *.gcov *.gcda *.gcno
TAGS_FILES = parser.c

LINTFLAGS	= -u
LINT		= lint

# Allow HTML documentation access to the units database by creating symbolic
# links in the webpage directory to the database files.
$(DESTDIR)$(htmldir):
	mkdir -p $@
install-data-hook:	$(DESTDIR)$(htmldir)
	cd $(DESTDIR)$(htmldir) && \
	    rm -rf $(PACKAGE)*.xml && \
	    ln -s ../../$(PACKAGE)/*.xml .

SUFFIXES = .c .l

.c.ln:
	$(LINT.c) $(AM_CPPFLAGS) $(CPPFLAGS) -c $<

.c.i:
	$(CPP) $(AM_CPPFLAGS) $(CPPFLAGS) $< >$@

.c.gcov:
	gcov -o .libs/libudunits2_la-$<.o $<.c

if HAVE_CUNIT
check-2:	testUnits
	UDUNITS2_XML_PATH='$(srcdir)/udunits2.xml' ./testUnits

check-1:	testUnits-1
	UDUNITS2_XML_PATH='$(srcdir)/udunits2.xml' ./testUnits-1

debug:		debug-2

debug-2:	testUnits
	UDUNITS2_XML_PATH='$(srcdir)/udunits2.xml' \
	    $(LIBTOOL) --mode=execute gdb testUnits

debug-1:	testUnits-1
	UDUNITS2_XML_PATH='$(srcdir)/udunits2.xml' \
	    $(LIBTOOL) --mode=execute gdb testUnits-1

debug-decode-time:	test_decode_time
	UDUNITS2_XML_PATH='$(srcdir)/udunits2.xml' \
	    $(LIBTOOL) --mode=execute gdb test_decode_time

valgrind-2:	testUnits
	UDUNITS2_XML_PATH='$(srcdir)/udunits2.xml' \
	    $(LIBTOOL) --mode=execute valgrind --leak-check=yes testUnits

valgrind-1:	testUnits-1
	UDUNITS2_XML_PATH='$(srcdir)/udunits2.xml' \
	    $(LIBTOOL) --mode=execute valgrind --leak-check=yes testUnits-1

.PHONY:		check-1 check-2 debug debug-1 debug-2 valgrind-1 valgrind-2
endif

parser.o: scanner.c

