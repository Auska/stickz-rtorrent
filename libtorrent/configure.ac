AC_INIT(libtorrent, 0.13.8, sundell.software@gmail.com)

LT_INIT([disable-static])

dnl Find a better way to do this
AC_DEFINE(PEER_NAME, "-lt0D80-", Identifier that is part of the default peer id)
AC_DEFINE(PEER_VERSION, "lt\x0D\x80", 4 byte client and version identifier for DHT)

LIBTORRENT_CURRENT=21
LIBTORRENT_REVISION=0
LIBTORRENT_AGE=0

LIBTORRENT_INTERFACE_VERSION_INFO=$LIBTORRENT_CURRENT:$LIBTORRENT_REVISION:$LIBTORRENT_AGE
LIBTORRENT_INTERFACE_VERSION_NO=$LIBTORRENT_CURRENT.$LIBTORRENT_AGE.$LIBTORRENT_REVISION

AC_SUBST(LIBTORRENT_CURRENT)
AC_SUBST(LIBTORRENT_INTERFACE_VERSION_INFO)
AC_SUBST(LIBTORRENT_INTERFACE_VERSION_NO)

AM_INIT_AUTOMAKE
AC_CONFIG_HEADERS(config.h)

AC_PROG_CXX

AC_C_BIGENDIAN(
    AC_DEFINE(IS_BIG_ENDIAN, 1, Big endian),
    AC_DEFINE(IS_LITTLE_ENDIAN, 1, Little endian),
    AC_MSG_ERROR([Could not determine endianness])
)

AX_CXX_COMPILE_STDCXX(17, noext, mandatory)

RAK_CHECK_CFLAGS
RAK_CHECK_CXXFLAGS
RAK_ENABLE_DEBUG
RAK_ENABLE_EXTRA_DEBUG
RAK_ENABLE_WERROR

AC_SYS_LARGEFILE

TORRENT_ENABLE_ALIGNED
TORRENT_ENABLE_INTERRUPT_SOCKET

TORRENT_ENABLE_ARCH
TORRENT_WITH_SYSROOT

dnl TORRENT_WITH_XFS
TORRENT_WITHOUT_KQUEUE
TORRENT_WITHOUT_EPOLL
TORRENT_CHECK_FALLOCATE
TORRENT_CHECK_POSIX_FALLOCATE
TORRENT_WITH_ADDRESS_SPACE

TORRENT_WITHOUT_STATVFS
TORRENT_WITHOUT_STATFS
TORRENT_WITH_INOTIFY
TORRENT_WITH_UDNS

CC_ATTRIBUTE_VISIBILITY

AX_PTHREAD

PKG_CHECK_MODULES([ZLIB], [zlib])
PKG_CHECK_MODULES([CPPUNIT], [cppunit],, [no_cppunit="yes"])

CFLAGS="$PTHREAD_CFLAGS $ZLIB_CFLAGS $CFLAGS"
CXXFLAGS="$PTHREAD_CFLAGS $ZLIB_CFLAGS $CXXFLAGS"
LIBS="$PTHREAD_LIBS $ZLIB_CFLAGS $LIBS"

AC_ARG_ENABLE(openssl,
  [  --disable-openssl       Don't use OpenSSL's SHA1 implementation.],
  [
    if test "$enableval" = "yes"; then
dnl move to scripts.
      PKG_CHECK_MODULES(OPENSSL, libcrypto,
        CXXFLAGS="$CXXFLAGS $OPENSSL_CFLAGS";
        LIBS="$LIBS $OPENSSL_LIBS")

      AC_DEFINE(USE_OPENSSL, 1, Using OpenSSL.)
      AC_DEFINE(USE_OPENSSL_SHA, 1, Using OpenSSL's SHA1 implementation.)
      AC_CHECK_LIB([crypto], [DH_set0_pqg], [AC_DEFINE(USE_OPENSSL_1_1, 1, Using OpenSSL 1.1.)])

    else
      AC_DEFINE(USE_NSS_SHA, 1, Using Mozilla's SHA1 implementation.)
    fi
  ],[
    PKG_CHECK_MODULES(OPENSSL, libcrypto,
      CXXFLAGS="$CXXFLAGS $OPENSSL_CFLAGS";
      LIBS="$LIBS $OPENSSL_LIBS")

    AC_DEFINE(USE_OPENSSL, 1, Using OpenSSL.)
    AC_DEFINE(USE_OPENSSL_SHA, 1, Using OpenSSL's SHA1 implementation.)
    AC_CHECK_LIB([crypto], [DH_set0_pqg], [AC_DEFINE(USE_OPENSSL_1_1, 1, Using OpenSSL 1.1.)])
  ]
)

AC_ARG_ENABLE(cyrus-rc4,
  [  --enable-cyrus-rc4=PFX  Use Cyrus RC4 implementation.],
  [
    CXXFLAGS="$CXXFLAGS -I${enableval}/include";
    LIBS="$LIBS -lrc4 -L${enableval}/lib"
    AC_DEFINE(USE_CYRUS_RC4, 1, Using Cyrus RC4 implementation.)
  ]
)

AC_CHECK_FUNCS(posix_memalign)

TORRENT_CHECK_MADVISE()
TORRENT_CHECK_CACHELINE()
TORRENT_HOSTED_MODE()
TORRENT_CHECK_EXECINFO()
TORRENT_CHECK_PTHREAD_SETNAME_NP()
TORRENT_MINCORE()

TORRENT_DISABLE_INSTRUMENTATION

LIBTORRENT_LIBS="-ltorrent"
AC_SUBST(LIBTORRENT_LIBS)

LIBTORRENT_CFLAGS=""
AC_SUBST(LIBTORRENT_CFLAGS)

AC_DEFINE(HAVE_CONFIG_H, 1, true if config.h was included)

CC_ATTRIBUTE_UNUSED(
	AC_DEFINE([__UNUSED], [__attribute__((unused))], [Wrapper around unused attribute]),
	AC_DEFINE([__UNUSED], [], [Null-wrapper if unused attribute is unsupported])
)

AC_OUTPUT([
	libtorrent.pc
	Makefile
	src/Makefile
	src/torrent/Makefile
	src/torrent/data/Makefile
	src/torrent/download/Makefile
	src/torrent/net/Makefile
	src/torrent/peer/Makefile
	src/torrent/utils/Makefile
	src/data/Makefile
	src/dht/Makefile
	src/download/Makefile
	src/net/Makefile
	src/protocol/Makefile
	src/tracker/Makefile
	src/utils/Makefile
        test/Makefile
])
