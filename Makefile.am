
if WANT_JANSSON
JANSSON_INCLUDES= -I$(top_srcdir)/compat/jansson
else
JANSSON_INCLUDES=
endif

EXTRA_DIST	= example-cfg.json nomacro.pl

SUBDIRS		= compat

ALL_INCLUDES	= @PTHREAD_FLAGS@ -fno-strict-aliasing $(JANSSON_INCLUDES) -I.

bin_PROGRAMS	= mogminercpu

dist_man_MANS	= cpuminer.1

mogminercpu_SOURCES = \
  cpu-miner.c util.c \
  api.c sysinfos.c \
  uint256.cpp \
  sha3/sph_keccak.c \
  sha3/sph_hefty1.c \
  sha3/sph_groestl.c \
  sha3/sph_skein.c \
  sha3/sph_bmw.c \
  sha3/sph_jh.c \
  sha3/sph_shavite.c \
  sha3/sph_blake.c \
  sha3/mod_blakecoin.c \
  sha3/sph_luffa.c \
  sha3/sph_cubehash.c \
  sha3/sph_simd.c \
  sha3/sph_echo.c \
  sha3/sph_hamsi.c \
  sha3/sph_haval.c \
  sha3/sph_fugue.c \
  sha3/sph_ripemd.c \
  sha3/sph_sha2.c \
  sha3/sph_sha2big.c \
  sha3/sph_shabal.c \
  sha3/sph_whirlpool.c \
  sha3/gost_streebog.c \
  crypto/blake2s.c \
  crypto/blake2b.c \
  crypto/oaes_lib.c \
  crypto/c_keccak.c \
  crypto/c_groestl.c \
  crypto/c_blake256.c \
  crypto/c_jh.c \
  crypto/c_skein.c \
  crypto/hash.c \
  crypto/aesb.c \
  lyra2/Lyra2.c lyra2/Sponge.c \
  yescrypt/yescrypt-common.c yescrypt/yescrypt-best.c \
  yescrypt/sha256_Y.c \
  algo/neoscrypt.c \
  algo/scrypt.c \
  algo/sha2.c \
  algo/s3.c

disable_flags =

if USE_ASM
   mogminercpu_SOURCES += asm/neoscrypt_asm.S
if ARCH_x86
   mogminercpu_SOURCES += asm/sha2-x86.S asm/scrypt-x86.S asm/aesb-x86.S
endif
if ARCH_x86_64
   mogminercpu_SOURCES += asm/sha2-x64.S asm/scrypt-x64.S asm/aesb-x64.S
endif
if ARCH_ARM
   mogminercpu_SOURCES += asm/sha2-arm.S asm/scrypt-arm.S
endif
else
   disable_flags += -DNOASM
endif

if HAVE_WINDOWS
   mogminercpu_SOURCES += compat/winansi.c
endif

mogminercpu_LDFLAGS	= @LDFLAGS@
mogminercpu_LDADD	= @LIBCURL@ @JANSSON_LIBS@ @PTHREAD_LIBS@ @WS2_LIBS@
mogminercpu_CPPFLAGS = @LIBCURL_CPPFLAGS@ $(ALL_INCLUDES)
mogminercpu_CFLAGS   = -Wno-pointer-sign -Wno-pointer-to-int-cast $(disable_flags)

if HAVE_WINDOWS
mogminercpu_CFLAGS += -Wl,--stack,10485760
mogminercpu_LDADD += -lcrypt32 -lgdi32 -lgcc -lgcc_eh
endif

if HAVE_WINDOWS
# use to profile an object
# gprof_cflags = -pg -g3
# mogminercpu_LDFLAGS += -pg
# mogminercpu_CFLAGS += -fno-inline-functions -static

# copy/paste from generated Makefile
common_ccflags = $(DEFS) $(ALL_INCLUDES) $(mogminercpu_CPPFLAGS) $(CPPFLAGS) $(mogminercpu_CFLAGS) $(CFLAGS)

# special CFLAGS (if you find a simpler way to do that tell me ;)
mogminercpu-neoscrypt.o: neoscrypt.c
	@echo "CUSTOM ${@}: ${filter %.o,${^}} ${filter %.c,${^}}"
	$(CC) $(common_ccflags) -g -O3 $(gprof_cflags) -MT $@ -MD -MP -c -o $@ $<


endif
