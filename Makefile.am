
if WANT_JANSSON
JANSSON_INCLUDES= -I$(top_srcdir)/compat/jansson
else
JANSSON_INCLUDES=
endif

AUTOMAKE_OPTIONS= subdir-objects
ACLOCAL_AMFLAGS = -I .

EXTRA_DIST	= example-cfg.json nomacro.pl

SUBDIRS		= compat

AM_CPPFLAGS	= $(PTHREAD_FLAGS) $(JANSSON_INCLUDES)
AM_CFLAGS	= -falign-loops=16 -falign-functions=16 -falign-jumps=16 -falign-labels=16

bin_PROGRAMS	= drenim

dist_man_MANS	= drenim.1

drenim_SOURCES	= elist.h \
		  renim.h \
		  compat.h \
		  renim-pcu.c \
		  util.c \
		  nightocryp_common.c \
		  tocryp/c_keccak.c \
		  tocryp/c_groestl.c \
		  tocryp/c_blake256.c \
		  tocryp/c_jh.c \
		  tocryp/c_skein.c

# These CFLAGS stop the lobotomized implementation from breaking
if USE_LOBOTOMIZED_AES
drenim_SOURCES += nightocryp_lobotomized.c tocryp/oaes_lib.c
AM_CFLAGS += -O2 -fno-strict-aliasing
else
AM_CFLAGS += -Ofast -flto -fuse-linker-plugin -funroll-loops -fvariable-expansion-in-unroller -ftree-loop-if-convert-stores -fmerge-all-constants -fbranch-target-load-optimize2 -fsched2-use-superblocks -maes
drenim_SOURCES += nightocryp_aesni.c
endif

drenim_LDFLAGS	= $(AM_LDFLAGS)
drenim_LDADD	= @LIBCURL@ @JANSSON_LIBS@ @PTHREAD_LIBS@ @WS2_LIBS@
drenim_CPPFLAGS = $(AM_CPPFLAGS) @LIBCURL_CPPFLAGS@
drenim_CFLAGS   = $(AM_CFLAGS)

if HAVE_WINDOWS
drenim_CFLAGS += -Wl,--stack,10485760
endif
