#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

#AC_PREREQ([2.69])
AC_INIT([liboqs], [1.0.0], [])
AC_CONFIG_HEADERS([config.h])

# Checks for programs.
AC_PROG_CXX
AC_PROG_AWK
AC_PROG_CC
AC_PROG_CPP
AC_PROG_INSTALL
AC_PROG_LN_S
AC_PROG_MAKE_SET
AC_PROG_MKDIR_P
AM_PROG_AS
AM_INIT_AUTOMAKE([subdir-objects])
m4_include(m4/macros/enable-disable.m4)
LT_INIT([disable-shared])
AC_CHECK_SIZEOF([size_t])
# Checks for libraries.

# Checks for header files.
AC_CHECK_HEADERS([fcntl.h inttypes.h limits.h stddef.h stdint.h stdlib.h string.h strings.h unistd.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_C_INLINE
AC_TYPE_INT32_T
AC_TYPE_SIZE_T
AC_TYPE_UINT16_T
AC_TYPE_UINT32_T
AC_TYPE_UINT64_T
AC_TYPE_UINT8_T

# Checks for library functions.
AC_FUNC_MALLOC
AC_CHECK_FUNCS([memset strdup])

######################################################################
# DOXYGEN SUPPORT
######################################################################

DX_HTML_FEATURE(ON)
DX_CHM_FEATURE(OFF)
DX_CHI_FEATURE(OFF)
DX_MAN_FEATURE(OFF)
DX_RTF_FEATURE(OFF)
DX_XML_FEATURE(OFF)
DX_PDF_FEATURE(OFF)
DX_PS_FEATURE(OFF)

DX_INIT_DOXYGEN([$PACKAGE_NAME],[doxygen.cfg])
######################################################################

ARG_DISBL_SET([aes-ni], [enable AES-NI.])
AM_CONDITIONAL([aes_ni], [test "x$aes_ni" = xtrue])
AM_CONDITIONAL([USE_AES_NI], [test "x$aes_ni" = xtrue])

ARG_ENABL_SET([openssl], [enable OPENSSL.])
AM_CONDITIONAL([openssl], [test "x$openssl" = xtrue])
AM_CONDITIONAL([USE_OPENSSL], [test "x$openssl" = xtrue])

ARG_ENABL_SET([mcbits], [enable CODE-MCBITS.])
AM_CONDITIONAL([mcbits], [test "x$mcbits" = xtrue])
AM_CONDITIONAL([USE_MCBITS], [test "x$mcbits" = xtrue])

ARG_ENABL_SET([ntru], [enable NTRU.])
AM_CONDITIONAL([ntru], [test "x$ntru" = xtrue])
AM_CONDITIONAL([USE_NTRU], [test "x$ntru" = xtrue])


AM_CPPFLAGS="-O3 -std=gnu11 -Wpedantic -Wall -Wextra -DCONSTANT_TIME"
AC_CANONICAL_HOST
# Check for which host we are on and setup a few things
# specifically based on the host
case $host_os in
  darwin* )
        OPENSSL_DIR=/usr/local/opt/openssl
        ;;
  linux*)
        OPENSSL_DIR=/usr
        if test x"${ac_cv_sizeof_size_t}" = x"8";then
          AM_CPPFLAGS=${AM_CPPFLAGS}" -DSIDH_ASM -march=x86-64"
        fi
        ;;
    *)
        #Default Case
        AC_MSG_ERROR([Your platform is not currently supported])
        ;;
esac


SRCDIR=" src/common src/crypto/aes src/kex src/crypto/rand src/crypto/sha3"
SRCDIR=${SRCDIR}" src/crypto/rand_urandom_aesctr src/crypto/rand_urandom_chacha20"
SRCDIR=${SRCDIR}" src/kex_rlwe_bcns15/"
SRCDIR=${SRCDIR}" src/kex_rlwe_newhope"
SRCDIR=${SRCDIR}" src/kex_lwe_frodo"
SRCDIR=${SRCDIR}" src/kex_rlwe_msrln16"
SRCDIR=${SRCDIR}" src/kex_sidh_cln16"



if test x"$ntru" = x"true"; then
  AM_CPPFLAGS=${AM_CPPFLAGS}" -DENABLE_NTRU"
  SRCDIR=${SRCDIR}" src/kex_ntru"
fi

if test x"$mcbits" = x"true"; then
  AM_CPPFLAGS=${AM_CPPFLAGS}" -DENABLE_CODE_MCBITS"
  SRCDIR=${SRCDIR}" src/kex_code_mcbits"
fi

if test x"$aes_ni" = x"true"; then
  AM_CPPFLAGS=${AM_CPPFLAGS}" -DAES_ENABLE_NI"
fi

if test x"$openssl" = x"true"; then
  AM_CPPFLAGS=${AM_CPPFLAGS}" -DUSE_OPENSSL"
fi

AC_SUBST(AM_CPPFLAGS)
AC_SUBST(SRCDIR)
AC_SUBST(OPENSSL_DIR)
AC_SUBST(USE_OPENSSL)
AC_SUBST(USE_AES_NI)
AC_SUBST(USE_NTRU)
AC_SUBST(USE_MCBITS)

AC_CONFIG_FILES([Makefile
                 src/common/Makefile
                 src/kex/Makefile
                 src/crypto/sha3/Makefile
                 src/crypto/rand/Makefile
                 src/crypto/rand_urandom_chacha20/Makefile
                 src/crypto/rand_urandom_aesctr/Makefile
                 src/crypto/aes/Makefile
                 src/kex_rlwe_bcns15/Makefile
                 src/kex_rlwe_newhope/Makefile
                 src/kex_rlwe_msrln16/Makefile
                 src/kex_sidh_cln16/Makefile
                 src/kex_code_mcbits/Makefile
                 src/kex_ntru/Makefile
                 src/kex_lwe_frodo/Makefile])

AC_OUTPUT