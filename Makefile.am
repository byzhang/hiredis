## Copyright (c) 2012, Cornell University
## All rights reserved.
##
## Redistribution and use in source and binary forms, with or without
## modification, are permitted provided that the following conditions are met:
##
##     * Redistributions of source code must retain the above copyright notice,
##       this list of conditions and the following disclaimer.
##     * Redistributions in binary form must reproduce the above copyright
##       notice, this list of conditions and the following disclaimer in the
##       documentation and/or other materials provided with the distribution.
##     * Neither the name of HyperDex nor the names of its contributors may be
##       used to endorse or promote products derived from this software without
##       specific prior written permission.
##
## THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
## AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
## IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
## ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE
## LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
## CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
## SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
## INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
## CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
## ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
## POSSIBILITY OF SUCH DAMAGE.

ACLOCAL_AMFLAGS = -I m4 ${ACLOCAL_FLAGS}
AM_CXXFLAGS = $(COVERAGE_CXXFLAGS)

lib_LTLIBRARIES = \
			libhiredis.la 

bin_PROGRAMS = \
			hiredis_example_libevent 

.PHONY: coverage

################################################################################
#################################### Hiredis ###################################
################################################################################

#################################### Library ###################################

libhiredis_includedir = $(includedir)/hiredis
libhiredis_include_HEADERS = \
			hiredis.h \
			async.h 

libhiredis_noinst_headers = \
			fmacros.h \
			net.h \
			sds.h \
			dict.h

libhiredis_la_SOURCES = \
			async.c \
			dict.c \
			hiredis.c \
			example.c \
			sds.c \
			net.c
libhiredis_la_CPPFLAGS = \
			$(E_CFLAGS) \
			$(CPPFLAGS)
libhiredis_la_LIBADD = \
			$(E_LIBS) 

libhiredis_adapters_includedir = $(includedir)/hiredis/adapters
libhiredis_adapters_include_HEADERS = \
			adapters/ae.h \
			adapters/libevent.h \
			adapters/libev.h

################################################################################
################################### Binaries ###################################
################################################################################

hiredis_example_libevent_SOURCES = \
			example-libevent.c
hiredis_example_libevent_LDADD = \
			libhiredis.la \
			$(COVERAGE_LDADD) \
			-lrt -levent
hiredis_example_libevent_CPPFLAGS = \
			$(CPPFLAGS)
hiredis_example_libevent_LDFLAGS = \
			-no-install

# hiredis_example_libev_SOURCES = \
#                         example-libev.c
# hiredis_example_libev_LDADD = \
#                         libhiredis.la \
#                         $(COVERAGE_LDADD) \
#                         -lrt -lev
# hiredis_example_libev_CPPFLAGS = \
#                         $(CPPFLAGS)
# hiredis_example_libev_LDFLAGS = \
#                         -no-install

# hiredis_test_SOURCES = \
#                         test.c
# hiredis_test_LDADD = \
#                         libhiredis.la \
#                         $(COVERAGE_LDADD) \
#                         -lrt
# hiredis_test_CPPFLAGS = \
#                         $(CPPFLAGS)
# hiredis_test_LDFLAGS = \
#                         -no-install

################################################################################
################################### Coverage ###################################
################################################################################

if ENABLE_COVERAGE
coverage: coverage.dat
	mkdir -p coverage
	genhtml --function-coverage --branch-coverage --demangle-cpp --output coverage $(abs_top_builddir)/coverage.dat

coverage.dat: check
	lcov --directory $(abs_top_builddir) --compat-libtool --capture --output $(abs_top_builddir)/coverage.dat
	lcov --extract $(abs_top_builddir)/coverage.dat $(abs_top_builddir)/'*' --output $(abs_top_builddir)/coverage.dat

clean-coverage:
	rm -f coverage.dat
	rm -rf coverage/
	find $(abs_top_builddir) -type f -iname '*.gcda' -delete
	find $(abs_top_builddir) -type f -iname '*.gcno' -delete

clean-local: clean-coverage
else
coverage:
	@echo Coverage not configured.
endif
